---
title: Specifica di SSDL - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
ms.openlocfilehash: a8b1f844a34c44d283982a52cef3bf80afd7e679
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490350"
---
# <a name="ssdl-specification"></a><span data-ttu-id="23c98-102">Specifica SSDL</span><span class="sxs-lookup"><span data-stu-id="23c98-102">SSDL Specification</span></span>
<span data-ttu-id="23c98-103">Store Schema Definition Language (SSDL) è un linguaggio basato su XML che descrive il modello di archiviazione di un'applicazione Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="23c98-103">Store schema definition language (SSDL) is an XML-based language that describes the storage model of an Entity Framework application.</span></span>

<span data-ttu-id="23c98-104">In un'applicazione Entity Framework, i metadati del modello di archiviazione viene caricato da un file con estensione ssdl, scritto in SSDL, in un'istanza del System.Data.Metadata.Edm.StoreItemCollection e sia accessibili tramite i metodi di Classe System.Data.Metadata.Edm.MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="23c98-104">In an Entity Framework application, storage model metadata is loaded from a .ssdl file (written in SSDL) into an instance of the System.Data.Metadata.Edm.StoreItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="23c98-105">Entity Framework Usa i metadati del modello di archiviazione per tradurre le query sul modello concettuale in comandi specifici dell'archivio.</span><span class="sxs-lookup"><span data-stu-id="23c98-105">Entity Framework uses storage model metadata to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="23c98-106">Entity Framework Designer (Entity Framework Designer) archivia le informazioni sul modello di archiviazione in un file con estensione edmx in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="23c98-106">The Entity Framework Designer (EF Designer) stores storage model information in an .edmx file at design time.</span></span> <span data-ttu-id="23c98-107">In fase di compilazione Entity Designer utilizza le informazioni in un file con estensione edmx per creare il file con estensione SSDL che serve da Entity Framework in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="23c98-107">At build time the Entity Designer uses information in an .edmx file to create the .ssdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="23c98-108">Le versioni di SSDL si differenziano tra loro per gli spazi dei nomi XML.</span><span class="sxs-lookup"><span data-stu-id="23c98-108">Versions of SSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="23c98-109">Versione SSDL</span><span class="sxs-lookup"><span data-stu-id="23c98-109">SSDL Version</span></span> | <span data-ttu-id="23c98-110">Namespace XML</span><span class="sxs-lookup"><span data-stu-id="23c98-110">XML Namespace</span></span>                                     |
|:-------------|:--------------------------------------------------|
| <span data-ttu-id="23c98-111">SSDL v1</span><span class="sxs-lookup"><span data-stu-id="23c98-111">SSDL v1</span></span>      | http://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| <span data-ttu-id="23c98-112">SSDL v2</span><span class="sxs-lookup"><span data-stu-id="23c98-112">SSDL v2</span></span>      | http://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| <span data-ttu-id="23c98-113">SSDL v3</span><span class="sxs-lookup"><span data-stu-id="23c98-113">SSDL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a><span data-ttu-id="23c98-114">Elemento Association (SSDL)</span><span class="sxs-lookup"><span data-stu-id="23c98-114">Association Element (SSDL)</span></span>

<span data-ttu-id="23c98-115">Un' **Association** elemento schema store schema definition language (SSDL) consente di specificare le colonne di tabella che fanno parte di un vincolo di chiave esterna nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="23c98-115">An **Association** element in store schema definition language (SSDL) specifies table columns that participate in a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="23c98-116">Due elementi End figlio obbligatorio specificano tabelle alle estremità dell'associazione e la molteplicità in ciascuna estremità.</span><span class="sxs-lookup"><span data-stu-id="23c98-116">Two required child End elements specify tables at the ends of the association and the multiplicity at each end.</span></span> <span data-ttu-id="23c98-117">Elemento ReferentialConstraint facoltativo specifica le entità finali principale e dipendente dell'associazione, nonché le colonne coinvolte.</span><span class="sxs-lookup"><span data-stu-id="23c98-117">An optional ReferentialConstraint element specifies the principal and dependent ends of the association as well as the participating columns.</span></span> <span data-ttu-id="23c98-118">Se nessun **ReferentialConstraint** è presente l'elemento, un elemento AssociationSetMapping deve essere usato per specificare i mapping delle colonne per l'associazione.</span><span class="sxs-lookup"><span data-stu-id="23c98-118">If no **ReferentialConstraint** element is present, an AssociationSetMapping element must be used to specify the column mappings for the association.</span></span>

<span data-ttu-id="23c98-119">Il **Association** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="23c98-119">The **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="23c98-120">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="23c98-120">Documentation (zero or one)</span></span>
-   <span data-ttu-id="23c98-121">End (esattamente due elementi)</span><span class="sxs-lookup"><span data-stu-id="23c98-121">End (exactly two)</span></span>
-   <span data-ttu-id="23c98-122">ReferentialConstraint (zero o più)</span><span class="sxs-lookup"><span data-stu-id="23c98-122">ReferentialConstraint (zero or one)</span></span>
-   <span data-ttu-id="23c98-123">Elementi Annotation (zero o più)</span><span class="sxs-lookup"><span data-stu-id="23c98-123">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="23c98-124">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="23c98-124">Applicable Attributes</span></span>

<span data-ttu-id="23c98-125">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **Association** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-125">The following table describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="23c98-126">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="23c98-126">Attribute Name</span></span> | <span data-ttu-id="23c98-127">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="23c98-127">Is Required</span></span> | <span data-ttu-id="23c98-128">Valore</span><span class="sxs-lookup"><span data-stu-id="23c98-128">Value</span></span>                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| <span data-ttu-id="23c98-129">**Name**</span><span class="sxs-lookup"><span data-stu-id="23c98-129">**Name**</span></span>       | <span data-ttu-id="23c98-130">Yes</span><span class="sxs-lookup"><span data-stu-id="23c98-130">Yes</span></span>         | <span data-ttu-id="23c98-131">Il nome del vincolo di chiave esterna corrispondente nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="23c98-131">The name of the corresponding foreign key constraint in the underlying database.</span></span> |

> [!NOTE]
> <span data-ttu-id="23c98-132">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **Association** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-132">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="23c98-133">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="23c98-133">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="23c98-134">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="23c98-134">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="23c98-135">Esempio</span><span class="sxs-lookup"><span data-stu-id="23c98-135">Example</span></span>

<span data-ttu-id="23c98-136">L'esempio seguente mostra un' **Association** elemento che utilizza un **ReferentialConstraint** elemento per specificare le colonne che fanno parte del **FK\_CustomerOrders**  vincolo di chiave esterna:</span><span class="sxs-lookup"><span data-stu-id="23c98-136">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="associationset-element-ssdl"></a><span data-ttu-id="23c98-137">Elemento AssociationSet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="23c98-137">AssociationSet Element (SSDL)</span></span>

<span data-ttu-id="23c98-138">Il **AssociationSet** elemento schema store schema definition language (SSDL) rappresenta un vincolo di chiave esterna tra due tabelle nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="23c98-138">The **AssociationSet** element in store schema definition language (SSDL) represents a foreign key constraint between two tables in the underlying database.</span></span> <span data-ttu-id="23c98-139">Le colonne della tabella che fanno parte del vincolo di chiave esterna vengono specificate in un elemento di associazione.</span><span class="sxs-lookup"><span data-stu-id="23c98-139">The table columns that participate in the foreign key constraint are specified in an Association element.</span></span> <span data-ttu-id="23c98-140">Il **associazione** elemento che corrisponde a un determinato **AssociationSet** viene specificato nell'elemento il **associazione** attributo del **AssociationSet**  elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-140">The **Association** element that corresponds to a given **AssociationSet** element is specified in the **Association** attribute of the **AssociationSet** element.</span></span>

<span data-ttu-id="23c98-141">Set di associazioni SSDL viene eseguito il mapping al set di associazioni CSDL da un elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="23c98-141">SSDL association sets are mapped to CSDL association sets by an AssociationSetMapping element.</span></span> <span data-ttu-id="23c98-142">Tuttavia, se è impostata l'associazione CSDL per una determinata associazione CSDL è definito con un elemento ReferentialConstraint, non corrispondente **AssociationSetMapping** elemento è necessario.</span><span class="sxs-lookup"><span data-stu-id="23c98-142">However, if the CSDL association for a given CSDL association set is defined by using a ReferentialConstraint element , no corresponding **AssociationSetMapping** element is necessary.</span></span> <span data-ttu-id="23c98-143">In questo caso, se un' **AssociationSetMapping** l'elemento è presente, i mapping verranno sostituiti dalle **ReferentialConstraint** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-143">In this case, if an **AssociationSetMapping** element is present, the mappings it defines will be overridden by the **ReferentialConstraint** element.</span></span>

<span data-ttu-id="23c98-144">Il **AssociationSet** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="23c98-144">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="23c98-145">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="23c98-145">Documentation (zero or one)</span></span>
-   <span data-ttu-id="23c98-146">End (zero o due)</span><span class="sxs-lookup"><span data-stu-id="23c98-146">End (zero or two)</span></span>
-   <span data-ttu-id="23c98-147">Elementi Annotation (zero o più)</span><span class="sxs-lookup"><span data-stu-id="23c98-147">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="23c98-148">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="23c98-148">Applicable Attributes</span></span>

<span data-ttu-id="23c98-149">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **AssociationSet** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-149">The following table describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="23c98-150">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="23c98-150">Attribute Name</span></span>  | <span data-ttu-id="23c98-151">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="23c98-151">Is Required</span></span> | <span data-ttu-id="23c98-152">Valore</span><span class="sxs-lookup"><span data-stu-id="23c98-152">Value</span></span>                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="23c98-153">**Name**</span><span class="sxs-lookup"><span data-stu-id="23c98-153">**Name**</span></span>        | <span data-ttu-id="23c98-154">Yes</span><span class="sxs-lookup"><span data-stu-id="23c98-154">Yes</span></span>         | <span data-ttu-id="23c98-155">Nome del vincolo di chiave esterna rappresentato dal set di associazioni.</span><span class="sxs-lookup"><span data-stu-id="23c98-155">The name of the foreign key constraint that the association set represents.</span></span>                          |
| <span data-ttu-id="23c98-156">**Associazione**</span><span class="sxs-lookup"><span data-stu-id="23c98-156">**Association**</span></span> | <span data-ttu-id="23c98-157">Yes</span><span class="sxs-lookup"><span data-stu-id="23c98-157">Yes</span></span>         | <span data-ttu-id="23c98-158">Nome dell'associazione che definisce le colonne che fanno parte del vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="23c98-158">The name of the association that defines the columns that participate in the foreign key constraint.</span></span> |

> [!NOTE]
> <span data-ttu-id="23c98-159">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **AssociationSet** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-159">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="23c98-160">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="23c98-160">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="23c98-161">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="23c98-161">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="23c98-162">Esempio</span><span class="sxs-lookup"><span data-stu-id="23c98-162">Example</span></span>

<span data-ttu-id="23c98-163">L'esempio seguente mostra un' **AssociationSet** elemento che rappresenta il `FK_CustomerOrders` vincolo di chiave esterna nel database sottostante:</span><span class="sxs-lookup"><span data-stu-id="23c98-163">The following example shows an **AssociationSet** element that represents the `FK_CustomerOrders` foreign key constraint in the underlying database:</span></span>

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a><span data-ttu-id="23c98-164">Elemento CollectionType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="23c98-164">CollectionType Element (SSDL)</span></span>

<span data-ttu-id="23c98-165">Il **CollectionType** elemento di schema store schema definition language (SSDL) specifica che il tipo restituito della funzione è una raccolta.</span><span class="sxs-lookup"><span data-stu-id="23c98-165">The **CollectionType** element in store schema definition language (SSDL) specifies that a function’s return type is a collection.</span></span> <span data-ttu-id="23c98-166">Il **CollectionType** elemento è figlio dell'elemento ReturnType.</span><span class="sxs-lookup"><span data-stu-id="23c98-166">The **CollectionType** element is a child of the ReturnType element.</span></span> <span data-ttu-id="23c98-167">Il tipo di raccolta viene specificato usando l'elemento figlio di tipo di riga:</span><span class="sxs-lookup"><span data-stu-id="23c98-167">The type of collection is specified by using the RowType child element:</span></span>

> [!NOTE]
> <span data-ttu-id="23c98-168">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **CollectionType** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-168">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="23c98-169">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="23c98-169">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="23c98-170">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="23c98-170">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="23c98-171">Esempio</span><span class="sxs-lookup"><span data-stu-id="23c98-171">Example</span></span>

<span data-ttu-id="23c98-172">Nell'esempio seguente viene illustrata una funzione che usa un' **CollectionType** elemento per specificare che la funzione restituisce una raccolta di righe.</span><span class="sxs-lookup"><span data-stu-id="23c98-172">The following example shows a function that uses a **CollectionType** element to specify that the function returns a collection of rows.</span></span>

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

## <a name="commandtext-element-ssdl"></a><span data-ttu-id="23c98-173">Elemento CommandText (SSDL)</span><span class="sxs-lookup"><span data-stu-id="23c98-173">CommandText Element (SSDL)</span></span>

<span data-ttu-id="23c98-174">Il **CommandText** elemento schema store schema definition language (SSDL) è un figlio dell'elemento di funzione che consente di definire un'istruzione SQL che viene eseguita a livello di database.</span><span class="sxs-lookup"><span data-stu-id="23c98-174">The **CommandText** element in store schema definition language (SSDL) is a child of the Function element that allows you to define a SQL statement that is executed at the database.</span></span> <span data-ttu-id="23c98-175">Il **CommandText** elemento consente di aggiungere funzionalità simili a una stored procedure nel database, ma si definiscono le **CommandText** elemento nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="23c98-175">The **CommandText** element allows you to add functionality that is similar to a stored procedure in the database, but you define the **CommandText** element in the storage model.</span></span>

<span data-ttu-id="23c98-176">Il **CommandText** elemento non può avere elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="23c98-176">The **CommandText** element cannot have child elements.</span></span> <span data-ttu-id="23c98-177">Il corpo del **CommandText** elemento deve essere un'istruzione SQL valida per il database sottostante.</span><span class="sxs-lookup"><span data-stu-id="23c98-177">The body of the **CommandText** element must be a valid SQL statement for the underlying database.</span></span>

<span data-ttu-id="23c98-178">Attributi non sono applicabili per il **CommandText** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-178">No attributes are applicable to the **CommandText** element.</span></span>

### <a name="example"></a><span data-ttu-id="23c98-179">Esempio</span><span class="sxs-lookup"><span data-stu-id="23c98-179">Example</span></span>

<span data-ttu-id="23c98-180">L'esempio seguente mostra una **funzione** elemento con un elemento figlio **CommandText** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-180">The following example shows a **Function** element with a child **CommandText** element.</span></span> <span data-ttu-id="23c98-181">Esporre il **UpdateProductInOrder** fungere da un metodo in ObjectContext importandolo nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="23c98-181">Expose the **UpdateProductInOrder** function as a method on the ObjectContext by importing it into the conceptual model.</span></span>  

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

## <a name="definingquery-element-ssdl"></a><span data-ttu-id="23c98-182">Elemento DefiningQuery (SSDL)</span><span class="sxs-lookup"><span data-stu-id="23c98-182">DefiningQuery Element (SSDL)</span></span>

<span data-ttu-id="23c98-183">Il **DefiningQuery** elemento schema store schema definition language (SSDL) consente di eseguire un'istruzione SQL direttamente nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="23c98-183">The **DefiningQuery** element in store schema definition language (SSDL) allows you to execute a SQL statement directly in the underlying database.</span></span> <span data-ttu-id="23c98-184">Il **DefiningQuery** elemento viene comunemente usato, ad esempio una vista di database, ma la vista viene definita nel modello di archiviazione anziché nel database.</span><span class="sxs-lookup"><span data-stu-id="23c98-184">The **DefiningQuery** element is commonly used like a database view, but the view is defined in the storage model instead of the database.</span></span> <span data-ttu-id="23c98-185">La visualizzazione definita in un **DefiningQuery** elementi possono essere mappati a un tipo di entità nel modello concettuale tramite un elemento EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="23c98-185">The view defined in a **DefiningQuery** element can be mapped to an entity type in the conceptual model through an EntitySetMapping element.</span></span> <span data-ttu-id="23c98-186">Questi mapping sono di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="23c98-186">These mappings are read-only.</span></span>  

<span data-ttu-id="23c98-187">La sintassi SSDL seguente illustra la dichiarazione di un **EntitySet** aggiungendo il **DefiningQuery** elemento contenente una query usata per recuperare la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="23c98-187">The following SSDL syntax shows the declaration of an **EntitySet** followed by the **DefiningQuery** element that contains a query used to retrieve the view.</span></span>

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

<span data-ttu-id="23c98-188">È possibile usare le stored procedure in Entity Framework per abilitare scenari di lettura / scrittura sulle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="23c98-188">You can use stored procedures in the Entity Framework to enable read-write scenarios over views.</span></span> <span data-ttu-id="23c98-189">È possibile utilizzare una vista origine dati o una visualizzazione Entity SQL come tabella di base per il recupero dei dati e per l'elaborazione tramite le stored procedure delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="23c98-189">You can use either a data source view or an Entity SQL view as the base table for retrieving data and for change processing by stored procedures.</span></span>

<span data-ttu-id="23c98-190">È possibile usare la **DefiningQuery** elemento destinazione Microsoft SQL Server Compact 3.5.</span><span class="sxs-lookup"><span data-stu-id="23c98-190">You can use the **DefiningQuery** element to target Microsoft SQL Server Compact 3.5.</span></span> <span data-ttu-id="23c98-191">Anche se SQL Server Compact 3.5 non supporta stored procedure, è possibile implementare una funzionalità simile con il **DefiningQuery** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-191">Though SQL Server Compact 3.5 does not support stored procedures, you can implement similar functionality with the **DefiningQuery** element.</span></span> <span data-ttu-id="23c98-192">Un'altra situazione in cui può essere utile è nella creazione di stored procedure per risolvere una mancata corrispondenza tra i tipi di dati utilizzati nel linguaggio di programmazione e quelli dell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="23c98-192">Another place where it can be useful is in creating stored procedures to overcome a mismatch between the data types used in the programming language and those of the data source.</span></span> <span data-ttu-id="23c98-193">È possibile scrivere un **DefiningQuery** che accetta un determinato set di parametri e quindi chiama una stored procedure con un diverso set di parametri, ad esempio, una stored procedure che elimina i dati.</span><span class="sxs-lookup"><span data-stu-id="23c98-193">You could write a **DefiningQuery** that takes a certain set of parameters and then calls a stored procedure with a different set of parameters, for example, a stored procedure that deletes data.</span></span>

## <a name="dependent-element-ssdl"></a><span data-ttu-id="23c98-194">Elemento Dependent (SSDL)</span><span class="sxs-lookup"><span data-stu-id="23c98-194">Dependent Element (SSDL)</span></span>

<span data-ttu-id="23c98-195">Il **dipendenti** in schema store schema definition language (SSDL) è un elemento figlio all'elemento ReferentialConstraint che definisce l'estremità dipendente del vincolo di chiave esterno (chiamato anche vincolo referenziale).</span><span class="sxs-lookup"><span data-stu-id="23c98-195">The **Dependent** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the dependent end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="23c98-196">Il **dipendenti** elemento specifica la colonna (o le colonne) in una tabella che fanno riferimento a una colonna chiave primaria (o colonne).</span><span class="sxs-lookup"><span data-stu-id="23c98-196">The **Dependent** element specifies the column (or columns) in a table that reference a primary key column (or columns).</span></span> <span data-ttu-id="23c98-197">**PropertyRef** elementi specificano quali colonne fanno riferimento.</span><span class="sxs-lookup"><span data-stu-id="23c98-197">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="23c98-198">Il principale elemento specifica le colonne chiave primaria cui fanno riferimento le colonne specificate nel **dipendenti** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-198">The Principal element specifies the primary key columns that are referenced by columns that are specified in the **Dependent** element.</span></span>

<span data-ttu-id="23c98-199">Il **dipendenti** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="23c98-199">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="23c98-200">PropertyRef (una o più)</span><span class="sxs-lookup"><span data-stu-id="23c98-200">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="23c98-201">Elementi Annotation (zero o più)</span><span class="sxs-lookup"><span data-stu-id="23c98-201">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="23c98-202">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="23c98-202">Applicable Attributes</span></span>

<span data-ttu-id="23c98-203">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **dipendenti** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-203">The following table describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="23c98-204">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="23c98-204">Attribute Name</span></span> | <span data-ttu-id="23c98-205">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="23c98-205">Is Required</span></span> | <span data-ttu-id="23c98-206">Valore</span><span class="sxs-lookup"><span data-stu-id="23c98-206">Value</span></span>                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="23c98-207">**Ruolo**</span><span class="sxs-lookup"><span data-stu-id="23c98-207">**Role**</span></span>       | <span data-ttu-id="23c98-208">Yes</span><span class="sxs-lookup"><span data-stu-id="23c98-208">Yes</span></span>         | <span data-ttu-id="23c98-209">Lo stesso valore di **ruolo** (se utilizzato) dell'attributo dell'elemento End corrispondente; in caso contrario, il nome della tabella contenente la colonna di riferimento.</span><span class="sxs-lookup"><span data-stu-id="23c98-209">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referencing column.</span></span> |

> [!NOTE]
> <span data-ttu-id="23c98-210">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **dipendenti** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-210">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="23c98-211">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="23c98-211">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="23c98-212">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="23c98-212">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="23c98-213">Esempio</span><span class="sxs-lookup"><span data-stu-id="23c98-213">Example</span></span>

<span data-ttu-id="23c98-214">L'esempio seguente illustra un elemento di associazione che utilizza un **ReferentialConstraint** elemento per specificare le colonne che partecipano le **FK\_CustomerOrders** chiave esterna vincolo.</span><span class="sxs-lookup"><span data-stu-id="23c98-214">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="23c98-215">Il **dipendenti** elemento specifica il **CustomerId** colonna del **ordine** tabella come entità finale dipendente del vincolo.</span><span class="sxs-lookup"><span data-stu-id="23c98-215">The **Dependent** element specifies the **CustomerId** column of the **Order** table as the dependent end of the constraint.</span></span>

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

## <a name="documentation-element-ssdl"></a><span data-ttu-id="23c98-216">Elemento Documentation (SSDL)</span><span class="sxs-lookup"><span data-stu-id="23c98-216">Documentation Element (SSDL)</span></span>

<span data-ttu-id="23c98-217">Il **documentazione** elemento in schema store schema definition language (SSDL) può essere usato per fornire informazioni su un oggetto definito in un elemento padre.</span><span class="sxs-lookup"><span data-stu-id="23c98-217">The **Documentation** element in store schema definition language (SSDL) can be used to provide information about an object that is defined in a parent element.</span></span>

<span data-ttu-id="23c98-218">Il **documentazione** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="23c98-218">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="23c98-219">**Riepilogo**: una breve descrizione dell'elemento padre.</span><span class="sxs-lookup"><span data-stu-id="23c98-219">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="23c98-220">Zero o un elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-220">(zero or one element)</span></span>
-   <span data-ttu-id="23c98-221">**LongDescription**: descrizione Luna dell'elemento padre.</span><span class="sxs-lookup"><span data-stu-id="23c98-221">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="23c98-222">Zero o un elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-222">(zero or one element)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="23c98-223">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="23c98-223">Applicable Attributes</span></span>

<span data-ttu-id="23c98-224">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **documentazione** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-224">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="23c98-225">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="23c98-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="23c98-226">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="23c98-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="23c98-227">Esempio</span><span class="sxs-lookup"><span data-stu-id="23c98-227">Example</span></span>

<span data-ttu-id="23c98-228">L'esempio seguente mostra le **documentazione** elemento come elemento figlio di un elemento EntityType.</span><span class="sxs-lookup"><span data-stu-id="23c98-228">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span>

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

## <a name="end-element-ssdl"></a><span data-ttu-id="23c98-229">Elemento End (SSDL)</span><span class="sxs-lookup"><span data-stu-id="23c98-229">End Element (SSDL)</span></span>

<span data-ttu-id="23c98-230">Il **End** elemento di schema store schema definition language (SSDL) specifica la tabella e il numero di righe in un'entità finale del vincolo di chiave esterno nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="23c98-230">The **End** element in store schema definition language (SSDL) specifies the table and number of rows at one end of a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="23c98-231">Il **End** elemento può essere un figlio dell'elemento di associazione o l'elemento AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="23c98-231">The **End** element can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="23c98-232">In entrambi i casi, gli elementi figlio possibili e gli attributi applicabili sono diversi.</span><span class="sxs-lookup"><span data-stu-id="23c98-232">In each case, the possible child elements and applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="23c98-233">Elemento End come figlio dell'elemento Association</span><span class="sxs-lookup"><span data-stu-id="23c98-233">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="23c98-234">Un' **End** elemento (come figlio del **Association** elemento) specifica la tabella e il numero di righe alla fine di un vincolo di chiave esterna con il **tipo** e**Molteplicità** rispettivamente gli attributi.</span><span class="sxs-lookup"><span data-stu-id="23c98-234">An **End** element (as a child of the **Association** element) specifies the table and number of rows at the end of a foreign key constraint with the **Type** and **Multiplicity** attributes respectively.</span></span> <span data-ttu-id="23c98-235">Le entità finali del vincolo di chiave esterna sono definite come parte dell'associazione SSDL. L'associazione SSDL deve disporre esattamente di due entità finali.</span><span class="sxs-lookup"><span data-stu-id="23c98-235">Ends of a foreign key constraint are defined as part of an SSDL association; an SSDL association must have exactly two ends.</span></span>

<span data-ttu-id="23c98-236">Un' **End** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="23c98-236">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="23c98-237">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="23c98-237">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="23c98-238">OnDelete (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="23c98-238">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="23c98-239">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="23c98-239">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="23c98-240">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="23c98-240">Applicable Attributes</span></span>

<span data-ttu-id="23c98-241">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **finali** elemento quando è figlio di un **associazione** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-241">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="23c98-242">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="23c98-242">Attribute Name</span></span>   | <span data-ttu-id="23c98-243">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="23c98-243">Is Required</span></span> | <span data-ttu-id="23c98-244">Valore</span><span class="sxs-lookup"><span data-stu-id="23c98-244">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="23c98-245">**Type**</span><span class="sxs-lookup"><span data-stu-id="23c98-245">**Type**</span></span>         | <span data-ttu-id="23c98-246">Yes</span><span class="sxs-lookup"><span data-stu-id="23c98-246">Yes</span></span>         | <span data-ttu-id="23c98-247">Il nome completo del set di entità SSDL che si trova alla fine del vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="23c98-247">The fully qualified name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                                                                                                                                                                                                                                                                          |
| <span data-ttu-id="23c98-248">**Ruolo**</span><span class="sxs-lookup"><span data-stu-id="23c98-248">**Role**</span></span>         | <span data-ttu-id="23c98-249">No</span><span class="sxs-lookup"><span data-stu-id="23c98-249">No</span></span>          | <span data-ttu-id="23c98-250">Il valore della **ruolo** attributo nell'elemento principale o dipendente del corrispondente elemento ReferentialConstraint (se usato).</span><span class="sxs-lookup"><span data-stu-id="23c98-250">The value of the **Role** attribute in either the Principal or Dependent element of the corresponding ReferentialConstraint element (if used).</span></span>                                                                                                                                                                                                                                             |
| <span data-ttu-id="23c98-251">**Molteplicità**</span><span class="sxs-lookup"><span data-stu-id="23c98-251">**Multiplicity**</span></span> | <span data-ttu-id="23c98-252">Yes</span><span class="sxs-lookup"><span data-stu-id="23c98-252">Yes</span></span>         | <span data-ttu-id="23c98-253">**1**, **0..1**, o **\*** a seconda del numero di righe che possono essere alla fine del vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="23c98-253">**1**, **0..1**, or **\*** depending on the number of rows that can be at the end of the foreign key constraint.</span></span> <br/> <span data-ttu-id="23c98-254">**1** indica esattamente una riga presente alla fine vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="23c98-254">**1** indicates that exactly one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="23c98-255">**0..1** indica che una o nessuna riga presente alla fine vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="23c98-255">**0..1** indicates that zero or one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="23c98-256">**\*** indica che esistano zero, uno o più righe alla fine vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="23c98-256">**\*** indicates that zero, one, or more rows exist at the foreign key constraint end.</span></span> |

> [!NOTE]
> <span data-ttu-id="23c98-257">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **End** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-257">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="23c98-258">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="23c98-258">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="23c98-259">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="23c98-259">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="23c98-260">Esempio</span><span class="sxs-lookup"><span data-stu-id="23c98-260">Example</span></span>

<span data-ttu-id="23c98-261">L'esempio seguente mostra un' **Association** elemento che definisce il **FK\_CustomerOrders** vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="23c98-261">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="23c98-262">Il **molteplicità** i valori specificati in ogni **End** elemento indicano che molte righe nel **ordini** può essere associata a una riga nella tabella il **clienti**  tabella, ma solo una riga nella **clienti** può essere associata a una riga nella tabella il **ordini** tabella.</span><span class="sxs-lookup"><span data-stu-id="23c98-262">The **Multiplicity** values specified on each **End** element indicate that many rows in the **Orders** table can be associated with a row in the **Customers** table, but only one row in the **Customers** table can be associated with a row in the **Orders** table.</span></span> <span data-ttu-id="23c98-263">Inoltre, il **OnDelete** elemento indica che tutte le righe nel **ordini** che fanno riferimento a una determinata riga nella tabella il **clienti** tabella verrà eliminata se la riga in il **clienti** tabella viene eliminata.</span><span class="sxs-lookup"><span data-stu-id="23c98-263">Additionally, the **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="23c98-264">Elemento End come figlio dell'elemento AssociationSet</span><span class="sxs-lookup"><span data-stu-id="23c98-264">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="23c98-265">Il **End** elemento (come figlio di **AssociationSet** elemento) specifica una tabella in un'entità finale del vincolo di chiave esterno nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="23c98-265">The **End** element (as a child of the **AssociationSet** element) specifies a table at one end of a foreign key constraint in the underlying database.</span></span>

<span data-ttu-id="23c98-266">Un' **End** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="23c98-266">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="23c98-267">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="23c98-267">Documentation (zero or one)</span></span>
-   <span data-ttu-id="23c98-268">Elementi Annotation (zero o più)</span><span class="sxs-lookup"><span data-stu-id="23c98-268">Annotation elements (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="23c98-269">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="23c98-269">Applicable Attributes</span></span>

<span data-ttu-id="23c98-270">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **finali** elemento quando è figlio di un **AssociationSet** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-270">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="23c98-271">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="23c98-271">Attribute Name</span></span> | <span data-ttu-id="23c98-272">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="23c98-272">Is Required</span></span> | <span data-ttu-id="23c98-273">Valore</span><span class="sxs-lookup"><span data-stu-id="23c98-273">Value</span></span>                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="23c98-274">**Elemento EntitySet**</span><span class="sxs-lookup"><span data-stu-id="23c98-274">**EntitySet**</span></span>  | <span data-ttu-id="23c98-275">Yes</span><span class="sxs-lookup"><span data-stu-id="23c98-275">Yes</span></span>         | <span data-ttu-id="23c98-276">Il nome del set di entità SSDL che si trova alla fine del vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="23c98-276">The name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                      |
| <span data-ttu-id="23c98-277">**Ruolo**</span><span class="sxs-lookup"><span data-stu-id="23c98-277">**Role**</span></span>       | <span data-ttu-id="23c98-278">No</span><span class="sxs-lookup"><span data-stu-id="23c98-278">No</span></span>          | <span data-ttu-id="23c98-279">Il valore di uno dei **ruolo** gli attributi specificati in uno **End** elemento dell'elemento di associazione corrispondente.</span><span class="sxs-lookup"><span data-stu-id="23c98-279">The value of one of the **Role** attributes specified on one **End** element of the corresponding Association element.</span></span> |

> [!NOTE]
> <span data-ttu-id="23c98-280">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **End** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-280">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="23c98-281">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="23c98-281">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="23c98-282">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="23c98-282">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="23c98-283">Esempio</span><span class="sxs-lookup"><span data-stu-id="23c98-283">Example</span></span>

<span data-ttu-id="23c98-284">L'esempio seguente mostra un' **EntityContainer** elemento con un **AssociationSet** elemento con due **End** elementi:</span><span class="sxs-lookup"><span data-stu-id="23c98-284">The following example shows an **EntityContainer** element with an **AssociationSet** element with two **End** elements:</span></span>

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

## <a name="entitycontainer-element-ssdl"></a><span data-ttu-id="23c98-285">Elemento EntityContainer (SSDL)</span><span class="sxs-lookup"><span data-stu-id="23c98-285">EntityContainer Element (SSDL)</span></span>

<span data-ttu-id="23c98-286">Un' **EntityContainer** elemento schema store schema definition language (SSDL) descrive la struttura dell'origine dati sottostante in un'applicazione Entity Framework: i set di entità SSDL (definiti negli elementi EntitySet) rappresentano le tabelle in un database, i tipi di entità SSDL (definiti negli elementi EntityType) rappresentano le righe in una tabella e set di associazioni (definiti negli elementi di AssociationSet) rappresentano vincoli di chiave esterna in un database.</span><span class="sxs-lookup"><span data-stu-id="23c98-286">An **EntityContainer** element in store schema definition language (SSDL) describes the structure of the underlying data source in an Entity Framework application: SSDL entity sets (defined in EntitySet elements) represent tables in a database, SSDL entity types (defined in EntityType elements) represent rows in a table, and association sets (defined in AssociationSet elements) represent foreign key constraints in a database.</span></span> <span data-ttu-id="23c98-287">Un contenitore di entità modello di archiviazione viene eseguito il mapping a un contenitore di entità del modello concettuale attraverso l'elemento EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="23c98-287">A storage model entity container maps to a conceptual model entity container through the EntityContainerMapping element.</span></span>

<span data-ttu-id="23c98-288">Un' **EntityContainer** elemento può avere zero o più elementi di documentazione.</span><span class="sxs-lookup"><span data-stu-id="23c98-288">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="23c98-289">Se un **documentazione** elemento è presente, deve precedere tutti gli elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="23c98-289">If a **Documentation** element is present, it must precede all other child elements.</span></span>

<span data-ttu-id="23c98-290">Un' **EntityContainer** elemento può includere zero o più degli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="23c98-290">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="23c98-291">EntitySet</span><span class="sxs-lookup"><span data-stu-id="23c98-291">EntitySet</span></span>
-   <span data-ttu-id="23c98-292">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="23c98-292">AssociationSet</span></span>
-   <span data-ttu-id="23c98-293">Elementi Annotation</span><span class="sxs-lookup"><span data-stu-id="23c98-293">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="23c98-294">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="23c98-294">Applicable Attributes</span></span>

<span data-ttu-id="23c98-295">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **EntityContainer** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-295">The table below describes the attributes that can be applied to the **EntityContainer** element.</span></span>

| <span data-ttu-id="23c98-296">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="23c98-296">Attribute Name</span></span> | <span data-ttu-id="23c98-297">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="23c98-297">Is Required</span></span> | <span data-ttu-id="23c98-298">Valore</span><span class="sxs-lookup"><span data-stu-id="23c98-298">Value</span></span>                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| <span data-ttu-id="23c98-299">**Name**</span><span class="sxs-lookup"><span data-stu-id="23c98-299">**Name**</span></span>       | <span data-ttu-id="23c98-300">Yes</span><span class="sxs-lookup"><span data-stu-id="23c98-300">Yes</span></span>         | <span data-ttu-id="23c98-301">Nome del contenitore di entità.</span><span class="sxs-lookup"><span data-stu-id="23c98-301">The name of the entity container.</span></span> <span data-ttu-id="23c98-302">Il nome non può contenere caratteri punto (.).</span><span class="sxs-lookup"><span data-stu-id="23c98-302">This name cannot contain periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="23c98-303">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **EntityContainer** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-303">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="23c98-304">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="23c98-304">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="23c98-305">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="23c98-305">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="23c98-306">Esempio</span><span class="sxs-lookup"><span data-stu-id="23c98-306">Example</span></span>

<span data-ttu-id="23c98-307">L'esempio seguente mostra un' **EntityContainer** elemento che definisce due set di entità e un set di associazioni.</span><span class="sxs-lookup"><span data-stu-id="23c98-307">The following example shows an **EntityContainer** element that defines two entity sets and one association set.</span></span> <span data-ttu-id="23c98-308">I nomi dei tipi di entità e associazione sono qualificati dal nome dello spazio dei nomi del modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="23c98-308">Note that entity type and association type names are qualified by the conceptual model namespace name.</span></span>

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

## <a name="entityset-element-ssdl"></a><span data-ttu-id="23c98-309">Elemento EntitySet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="23c98-309">EntitySet Element (SSDL)</span></span>

<span data-ttu-id="23c98-310">Un' **EntitySet** elemento schema store schema definition language (SSDL) rappresenta una tabella o vista nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="23c98-310">An **EntitySet** element in store schema definition language (SSDL) represents a table or view in the underlying database.</span></span> <span data-ttu-id="23c98-311">Un elemento EntityType nel linguaggio SSDL rappresenta una riga nella tabella o della vista.</span><span class="sxs-lookup"><span data-stu-id="23c98-311">An EntityType element in SSDL represents a row in the table or view.</span></span> <span data-ttu-id="23c98-312">Il **EntityType** attributo di un **EntitySet** elemento specifica il particolare tipo di entità SSDL che rappresenta le righe in un set di entità SSDL.</span><span class="sxs-lookup"><span data-stu-id="23c98-312">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="23c98-313">Il mapping tra un set di entità CSDL e un set di entità SSDL viene specificato in un elemento EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="23c98-313">The mapping between a CSDL entity set and an SSDL entity set is specified in an EntitySetMapping element.</span></span>

<span data-ttu-id="23c98-314">Il **EntitySet** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="23c98-314">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="23c98-315">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="23c98-315">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="23c98-316">DefiningQuery (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="23c98-316">DefiningQuery (zero or one element)</span></span>
-   <span data-ttu-id="23c98-317">Elementi Annotation</span><span class="sxs-lookup"><span data-stu-id="23c98-317">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="23c98-318">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="23c98-318">Applicable Attributes</span></span>

<span data-ttu-id="23c98-319">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **EntitySet** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-319">The following table describes the attributes that can be applied to the **EntitySet** element.</span></span>

> [!NOTE]
> <span data-ttu-id="23c98-320">Alcuni attributi (non elencati qui) possono essere qualificati con il **archiviare** alias.</span><span class="sxs-lookup"><span data-stu-id="23c98-320">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="23c98-321">Questi attributi vengono utilizzati per la procedura guidata Aggiorna modello quando si aggiorna un modello.</span><span class="sxs-lookup"><span data-stu-id="23c98-321">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="23c98-322">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="23c98-322">Attribute Name</span></span> | <span data-ttu-id="23c98-323">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="23c98-323">Is Required</span></span> | <span data-ttu-id="23c98-324">Valore</span><span class="sxs-lookup"><span data-stu-id="23c98-324">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="23c98-325">**Name**</span><span class="sxs-lookup"><span data-stu-id="23c98-325">**Name**</span></span>       | <span data-ttu-id="23c98-326">Yes</span><span class="sxs-lookup"><span data-stu-id="23c98-326">Yes</span></span>         | <span data-ttu-id="23c98-327">Nome del set di entità.</span><span class="sxs-lookup"><span data-stu-id="23c98-327">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="23c98-328">**Elemento EntityType**</span><span class="sxs-lookup"><span data-stu-id="23c98-328">**EntityType**</span></span> | <span data-ttu-id="23c98-329">Yes</span><span class="sxs-lookup"><span data-stu-id="23c98-329">Yes</span></span>         | <span data-ttu-id="23c98-330">Nome completo del tipo di entità per il quale il set di entità contiene delle istanze.</span><span class="sxs-lookup"><span data-stu-id="23c98-330">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |
| <span data-ttu-id="23c98-331">**Schema**</span><span class="sxs-lookup"><span data-stu-id="23c98-331">**Schema**</span></span>     | <span data-ttu-id="23c98-332">No</span><span class="sxs-lookup"><span data-stu-id="23c98-332">No</span></span>          | <span data-ttu-id="23c98-333">Schema del database.</span><span class="sxs-lookup"><span data-stu-id="23c98-333">The database schema.</span></span>                                                                     |
| <span data-ttu-id="23c98-334">**Tabella**</span><span class="sxs-lookup"><span data-stu-id="23c98-334">**Table**</span></span>      | <span data-ttu-id="23c98-335">No</span><span class="sxs-lookup"><span data-stu-id="23c98-335">No</span></span>          | <span data-ttu-id="23c98-336">Tabella del database.</span><span class="sxs-lookup"><span data-stu-id="23c98-336">The database table.</span></span>                                                                      |

> [!NOTE]
> <span data-ttu-id="23c98-337">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **EntitySet** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-337">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="23c98-338">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="23c98-338">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="23c98-339">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="23c98-339">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="23c98-340">Esempio</span><span class="sxs-lookup"><span data-stu-id="23c98-340">Example</span></span>

<span data-ttu-id="23c98-341">L'esempio seguente mostra un' **EntityContainer** elemento che dispone di due **EntitySet** elementi e uno **AssociationSet** elemento:</span><span class="sxs-lookup"><span data-stu-id="23c98-341">The following example shows an **EntityContainer** element that has two **EntitySet** elements and one **AssociationSet** element:</span></span>

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

## <a name="entitytype-element-ssdl"></a><span data-ttu-id="23c98-342">Elemento EntityType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="23c98-342">EntityType Element (SSDL)</span></span>

<span data-ttu-id="23c98-343">Un' **EntityType** elemento schema store schema definition language (SSDL) rappresenta una riga in una tabella o vista del database sottostante.</span><span class="sxs-lookup"><span data-stu-id="23c98-343">An **EntityType** element in store schema definition language (SSDL) represents a row in a table or view of the underlying database.</span></span> <span data-ttu-id="23c98-344">Un elemento EntitySet in SSDL rappresenta la tabella o vista in cui si trova la riga.</span><span class="sxs-lookup"><span data-stu-id="23c98-344">An EntitySet element in SSDL represents the table or view in which rows occur.</span></span> <span data-ttu-id="23c98-345">Il **EntityType** attributo di un **EntitySet** elemento specifica il particolare tipo di entità SSDL che rappresenta le righe in un set di entità SSDL.</span><span class="sxs-lookup"><span data-stu-id="23c98-345">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="23c98-346">Il mapping tra un tipo di entità SSDL e un tipo di entità CSDL viene specificato in un elemento EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="23c98-346">The mapping between an SSDL entity type and a CSDL entity type is specified in an EntityTypeMapping element.</span></span>

<span data-ttu-id="23c98-347">Il **EntityType** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="23c98-347">The **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="23c98-348">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="23c98-348">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="23c98-349">Chiave (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="23c98-349">Key (zero or one element)</span></span>
-   <span data-ttu-id="23c98-350">Elementi Annotation</span><span class="sxs-lookup"><span data-stu-id="23c98-350">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="23c98-351">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="23c98-351">Applicable Attributes</span></span>

<span data-ttu-id="23c98-352">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **EntityType** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-352">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="23c98-353">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="23c98-353">Attribute Name</span></span> | <span data-ttu-id="23c98-354">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="23c98-354">Is Required</span></span> | <span data-ttu-id="23c98-355">Valore</span><span class="sxs-lookup"><span data-stu-id="23c98-355">Value</span></span>                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="23c98-356">**Name**</span><span class="sxs-lookup"><span data-stu-id="23c98-356">**Name**</span></span>       | <span data-ttu-id="23c98-357">Yes</span><span class="sxs-lookup"><span data-stu-id="23c98-357">Yes</span></span>         | <span data-ttu-id="23c98-358">Nome del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="23c98-358">The name of the entity type.</span></span> <span data-ttu-id="23c98-359">Questo valore corrisponde generalmente al nome della tabella in cui il tipo di entità rappresenta una riga.</span><span class="sxs-lookup"><span data-stu-id="23c98-359">This value is usually the same as the name of the table in which the entity type represents a row.</span></span> <span data-ttu-id="23c98-360">Questo valore non può contenere punti (.).</span><span class="sxs-lookup"><span data-stu-id="23c98-360">This value can contain no periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="23c98-361">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **EntityType** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-361">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="23c98-362">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="23c98-362">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="23c98-363">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="23c98-363">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="23c98-364">Esempio</span><span class="sxs-lookup"><span data-stu-id="23c98-364">Example</span></span>

<span data-ttu-id="23c98-365">L'esempio seguente mostra un' **EntityType** elemento con due proprietà:</span><span class="sxs-lookup"><span data-stu-id="23c98-365">The following example shows an **EntityType** element with two properties:</span></span>

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

## <a name="function-element-ssdl"></a><span data-ttu-id="23c98-366">Elemento Function (SSDL)</span><span class="sxs-lookup"><span data-stu-id="23c98-366">Function Element (SSDL)</span></span>

<span data-ttu-id="23c98-367">Il **funzione** elemento di schema store schema definition language (SSDL) specifica una stored procedure esistente nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="23c98-367">The **Function** element in store schema definition language (SSDL) specifies a stored procedure that exists in the underlying database.</span></span>

<span data-ttu-id="23c98-368">Il **funzione** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="23c98-368">The **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="23c98-369">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="23c98-369">Documentation (zero or one)</span></span>
-   <span data-ttu-id="23c98-370">Parametro (zero o più)</span><span class="sxs-lookup"><span data-stu-id="23c98-370">Parameter (zero or more)</span></span>
-   <span data-ttu-id="23c98-371">CommandText (zero o più)</span><span class="sxs-lookup"><span data-stu-id="23c98-371">CommandText (zero or one)</span></span>
-   <span data-ttu-id="23c98-372">ReturnType (zero o più)</span><span class="sxs-lookup"><span data-stu-id="23c98-372">ReturnType (zero or more)</span></span>
-   <span data-ttu-id="23c98-373">Elementi Annotation (zero o più)</span><span class="sxs-lookup"><span data-stu-id="23c98-373">Annotation elements (zero or more)</span></span>

<span data-ttu-id="23c98-374">Restituisce un tipo per una funzione deve essere specificato mediante il **ReturnType** elemento o il **ReturnType** attributo (vedere sotto), ma non entrambi.</span><span class="sxs-lookup"><span data-stu-id="23c98-374">A return type for a function must be specified with either the **ReturnType** element or the **ReturnType** attribute (see below), but not both.</span></span>

<span data-ttu-id="23c98-375">È possibile importare stored procedure specificate nel modello di archiviazione nel modello concettuale di un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="23c98-375">Stored procedures that are specified in the storage model can be imported into the conceptual model of an application.</span></span> <span data-ttu-id="23c98-376">Per altre informazioni, vedere [esecuzione di query con Stored procedure](~/ef6/modeling/designer/stored-procedures/query.md).</span><span class="sxs-lookup"><span data-stu-id="23c98-376">For more information, see [Querying with Stored Procedures](~/ef6/modeling/designer/stored-procedures/query.md).</span></span> <span data-ttu-id="23c98-377">Il **funzione** elemento può essere usato anche per definire funzioni personalizzate nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="23c98-377">The **Function** element can also be used to define custom functions in the storage model.</span></span>  

### <a name="applicable-attributes"></a><span data-ttu-id="23c98-378">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="23c98-378">Applicable Attributes</span></span>

<span data-ttu-id="23c98-379">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **funzione** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-379">The following table describes the attributes that can be applied to the **Function** element.</span></span>

> [!NOTE]
> <span data-ttu-id="23c98-380">Alcuni attributi (non elencati qui) possono essere qualificati con il **archiviare** alias.</span><span class="sxs-lookup"><span data-stu-id="23c98-380">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="23c98-381">Questi attributi vengono utilizzati per la procedura guidata Aggiorna modello quando si aggiorna un modello.</span><span class="sxs-lookup"><span data-stu-id="23c98-381">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="23c98-382">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="23c98-382">Attribute Name</span></span>             | <span data-ttu-id="23c98-383">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="23c98-383">Is Required</span></span> | <span data-ttu-id="23c98-384">Valore</span><span class="sxs-lookup"><span data-stu-id="23c98-384">Value</span></span>                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="23c98-385">**Name**</span><span class="sxs-lookup"><span data-stu-id="23c98-385">**Name**</span></span>                   | <span data-ttu-id="23c98-386">Yes</span><span class="sxs-lookup"><span data-stu-id="23c98-386">Yes</span></span>         | <span data-ttu-id="23c98-387">Nome della stored procedure.</span><span class="sxs-lookup"><span data-stu-id="23c98-387">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="23c98-388">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="23c98-388">**ReturnType**</span></span>             | <span data-ttu-id="23c98-389">No</span><span class="sxs-lookup"><span data-stu-id="23c98-389">No</span></span>          | <span data-ttu-id="23c98-390">Tipo restituito della stored procedure.</span><span class="sxs-lookup"><span data-stu-id="23c98-390">The return type of the stored procedure.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="23c98-391">**Aggregate**</span><span class="sxs-lookup"><span data-stu-id="23c98-391">**Aggregate**</span></span>              | <span data-ttu-id="23c98-392">No</span><span class="sxs-lookup"><span data-stu-id="23c98-392">No</span></span>          | <span data-ttu-id="23c98-393">**True** se la stored procedure restituisce un valore di aggregazione; in caso contrario **False**.</span><span class="sxs-lookup"><span data-stu-id="23c98-393">**True** if the stored procedure returns an aggregate value; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="23c98-394">**BuiltIn**</span><span class="sxs-lookup"><span data-stu-id="23c98-394">**BuiltIn**</span></span>                | <span data-ttu-id="23c98-395">No</span><span class="sxs-lookup"><span data-stu-id="23c98-395">No</span></span>          | <span data-ttu-id="23c98-396">**True** se la funzione è un oggetto incorporato<sup>1</sup> funzione; in caso contrario **False**.</span><span class="sxs-lookup"><span data-stu-id="23c98-396">**True** if the function is a built-in<sup>1</sup> function; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="23c98-397">**StoreFunctionName**</span><span class="sxs-lookup"><span data-stu-id="23c98-397">**StoreFunctionName**</span></span>      | <span data-ttu-id="23c98-398">No</span><span class="sxs-lookup"><span data-stu-id="23c98-398">No</span></span>          | <span data-ttu-id="23c98-399">Nome della stored procedure.</span><span class="sxs-lookup"><span data-stu-id="23c98-399">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="23c98-400">**NiladicFunction**</span><span class="sxs-lookup"><span data-stu-id="23c98-400">**NiladicFunction**</span></span>        | <span data-ttu-id="23c98-401">No</span><span class="sxs-lookup"><span data-stu-id="23c98-401">No</span></span>          | <span data-ttu-id="23c98-402">**True** se la funzione è senza parametri<sup>2</sup> funzione; **False** in caso contrario.</span><span class="sxs-lookup"><span data-stu-id="23c98-402">**True** if the function is a niladic<sup>2</sup> function; **False** otherwise.</span></span>                                                                                                                                   |
| <span data-ttu-id="23c98-403">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="23c98-403">**IsComposable**</span></span>           | <span data-ttu-id="23c98-404">No</span><span class="sxs-lookup"><span data-stu-id="23c98-404">No</span></span>          | <span data-ttu-id="23c98-405">**True** se la funzione è componibile<sup>3</sup> funzione; **False** in caso contrario.</span><span class="sxs-lookup"><span data-stu-id="23c98-405">**True** if the function is a composable<sup>3</sup> function; **False** otherwise.</span></span>                                                                                                                                |
| <span data-ttu-id="23c98-406">**ParameterTypeSemantics**</span><span class="sxs-lookup"><span data-stu-id="23c98-406">**ParameterTypeSemantics**</span></span> | <span data-ttu-id="23c98-407">No</span><span class="sxs-lookup"><span data-stu-id="23c98-407">No</span></span>          | <span data-ttu-id="23c98-408">Enumerazione che definisce la semantica dei tipi utilizzata per risolvere gli overload della funzione.</span><span class="sxs-lookup"><span data-stu-id="23c98-408">The enumeration that defines the type semantics used to resolve function overloads.</span></span> <span data-ttu-id="23c98-409">L'enumerazione è definita nel file manifesto del provider per ogni definizione di funzione.</span><span class="sxs-lookup"><span data-stu-id="23c98-409">The enumeration is defined in the provider manifest per function definition.</span></span> <span data-ttu-id="23c98-410">Il valore predefinito è **AllowImplicitConversion**.</span><span class="sxs-lookup"><span data-stu-id="23c98-410">The default value is **AllowImplicitConversion**.</span></span> |
| <span data-ttu-id="23c98-411">**Schema**</span><span class="sxs-lookup"><span data-stu-id="23c98-411">**Schema**</span></span>                 | <span data-ttu-id="23c98-412">No</span><span class="sxs-lookup"><span data-stu-id="23c98-412">No</span></span>          | <span data-ttu-id="23c98-413">Nome dello schema in cui viene definita la stored procedure.</span><span class="sxs-lookup"><span data-stu-id="23c98-413">The name of the schema in which the stored procedure is defined.</span></span>                                                                                                                                                   |

<span data-ttu-id="23c98-414"><sup>1</sup> una funzione predefinita è una funzione definita nel database.</span><span class="sxs-lookup"><span data-stu-id="23c98-414"><sup>1</sup> A built-in function is a function that is defined in the database.</span></span> <span data-ttu-id="23c98-415">Per informazioni sulle funzioni definite nel modello di archiviazione, vedere l'elemento CommandText (SSDL).</span><span class="sxs-lookup"><span data-stu-id="23c98-415">For information about functions that are defined in the storage model, see CommandText Element (SSDL).</span></span>

<span data-ttu-id="23c98-416"><sup>2</sup> una funzione senza parametri è una funzione che non accetta parametri e, quando viene chiamato, non richiede le parentesi.</span><span class="sxs-lookup"><span data-stu-id="23c98-416"><sup>2</sup> A niladic function is a function that accepts no parameters and, when called, does not require parentheses.</span></span>

<span data-ttu-id="23c98-417"><sup>3</sup> due funzioni sono componibili se l'output di una funzione può essere l'input per l'altra funzione.</span><span class="sxs-lookup"><span data-stu-id="23c98-417"><sup>3</sup> Two functions are composable if the output of one function can be the input for the other function.</span></span>

> [!NOTE]
> <span data-ttu-id="23c98-418">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **funzione** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-418">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="23c98-419">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="23c98-419">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="23c98-420">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="23c98-420">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="23c98-421">Esempio</span><span class="sxs-lookup"><span data-stu-id="23c98-421">Example</span></span>

<span data-ttu-id="23c98-422">Nell'esempio seguente un **funzione** elemento che corrisponde al **UpdateOrderQuantity** stored procedure.</span><span class="sxs-lookup"><span data-stu-id="23c98-422">The following example shows a **Function** element that corresponds to the **UpdateOrderQuantity** stored procedure.</span></span> <span data-ttu-id="23c98-423">La stored procedure accetta due parametri e non restituisce alcun valore.</span><span class="sxs-lookup"><span data-stu-id="23c98-423">The stored procedure accepts two parameters and does not return a value.</span></span>

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

## <a name="key-element-ssdl"></a><span data-ttu-id="23c98-424">Elemento Key (SSDL)</span><span class="sxs-lookup"><span data-stu-id="23c98-424">Key Element (SSDL)</span></span>

<span data-ttu-id="23c98-425">Il **chiave** elemento schema store schema definition language (SSDL) rappresenta la chiave primaria di una tabella nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="23c98-425">The **Key** element in store schema definition language (SSDL) represents the primary key of a table in the underlying database.</span></span> <span data-ttu-id="23c98-426">**Chiave** è un elemento figlio di un elemento EntityType che rappresenta una riga in una tabella.</span><span class="sxs-lookup"><span data-stu-id="23c98-426">**Key** is a child element of an EntityType element, which represents a row in a table.</span></span> <span data-ttu-id="23c98-427">La chiave primaria è definita nel **Key** elemento facendo riferimento a uno o più elementi di proprietà definite nel **EntityType** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-427">The primary key is defined in the **Key** element by referencing one or more Property elements that are defined on the **EntityType** element.</span></span>

<span data-ttu-id="23c98-428">Il **chiave** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="23c98-428">The **Key** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="23c98-429">PropertyRef (una o più)</span><span class="sxs-lookup"><span data-stu-id="23c98-429">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="23c98-430">Elementi Annotation</span><span class="sxs-lookup"><span data-stu-id="23c98-430">Annotation elements</span></span>

<span data-ttu-id="23c98-431">Attributi non sono applicabili per il **chiave** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-431">No attributes are applicable to the **Key** element.</span></span>

### <a name="example"></a><span data-ttu-id="23c98-432">Esempio</span><span class="sxs-lookup"><span data-stu-id="23c98-432">Example</span></span>

<span data-ttu-id="23c98-433">L'esempio seguente mostra un' **EntityType** elemento con una chiave che fa riferimento a una proprietà:</span><span class="sxs-lookup"><span data-stu-id="23c98-433">The following example shows an **EntityType** element with a key that references one property:</span></span>

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

## <a name="ondelete-element-ssdl"></a><span data-ttu-id="23c98-434">Elemento OnDelete (SSDL)</span><span class="sxs-lookup"><span data-stu-id="23c98-434">OnDelete Element (SSDL)</span></span>

<span data-ttu-id="23c98-435">Il **OnDelete** elemento schema store schema definition language (SSDL) riflette il comportamento del database quando viene eliminata una riga che fa parte di un vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="23c98-435">The **OnDelete** element in store schema definition language (SSDL) reflects the database behavior when a row that participates in a foreign key constraint is deleted.</span></span> <span data-ttu-id="23c98-436">Se l'azione viene impostata su **Cascade**, quindi verranno eliminate anche le righe che fanno riferimento a una riga in fase di eliminazione.</span><span class="sxs-lookup"><span data-stu-id="23c98-436">If the action is set to **Cascade**, then rows that reference a row that is being deleted will also be deleted.</span></span> <span data-ttu-id="23c98-437">Se l'azione viene impostata su **None**, quindi non vengono eliminate anche le righe che fanno riferimento a una riga in fase di eliminazione.</span><span class="sxs-lookup"><span data-stu-id="23c98-437">If the action is set to **None**, then rows that reference a row that is being deleted are not also deleted.</span></span> <span data-ttu-id="23c98-438">Un' **OnDelete** è un elemento figlio dell'elemento finale.</span><span class="sxs-lookup"><span data-stu-id="23c98-438">An **OnDelete** element is a child element of an End element.</span></span>

<span data-ttu-id="23c98-439">Un' **OnDelete** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="23c98-439">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="23c98-440">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="23c98-440">Documentation (zero or one)</span></span>
-   <span data-ttu-id="23c98-441">Elementi Annotation (zero o più)</span><span class="sxs-lookup"><span data-stu-id="23c98-441">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="23c98-442">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="23c98-442">Applicable Attributes</span></span>

<span data-ttu-id="23c98-443">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **OnDelete** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-443">The following table describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="23c98-444">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="23c98-444">Attribute Name</span></span> | <span data-ttu-id="23c98-445">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="23c98-445">Is Required</span></span> | <span data-ttu-id="23c98-446">Valore</span><span class="sxs-lookup"><span data-stu-id="23c98-446">Value</span></span>                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="23c98-447">**Azione**</span><span class="sxs-lookup"><span data-stu-id="23c98-447">**Action**</span></span>     | <span data-ttu-id="23c98-448">Yes</span><span class="sxs-lookup"><span data-stu-id="23c98-448">Yes</span></span>         | <span data-ttu-id="23c98-449">**CASCADE** oppure **None**.</span><span class="sxs-lookup"><span data-stu-id="23c98-449">**Cascade** or **None**.</span></span> <span data-ttu-id="23c98-450">(Il valore **Restricted** è valido ma presenta lo stesso comportamento **None**.)</span><span class="sxs-lookup"><span data-stu-id="23c98-450">(The value **Restricted** is valid but has the same behavior as **None**.)</span></span> |

> [!NOTE]
> <span data-ttu-id="23c98-451">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **OnDelete** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-451">Any number of annotation attributes (custom XML attributes) may be applied to the **OnDelete** element.</span></span> <span data-ttu-id="23c98-452">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="23c98-452">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="23c98-453">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="23c98-453">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="23c98-454">Esempio</span><span class="sxs-lookup"><span data-stu-id="23c98-454">Example</span></span>

<span data-ttu-id="23c98-455">L'esempio seguente mostra un' **Association** elemento che definisce il **FK\_CustomerOrders** vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="23c98-455">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="23c98-456">Il **OnDelete** elemento indica che tutte le righe nel **ordini** che fanno riferimento a una determinata riga nella tabella di **clienti** tabella verrà eliminata se la riga nella finestra di **Clienti** tabella viene eliminata.</span><span class="sxs-lookup"><span data-stu-id="23c98-456">The **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

## <a name="parameter-element-ssdl"></a><span data-ttu-id="23c98-457">Elemento Parameter (SSDL)</span><span class="sxs-lookup"><span data-stu-id="23c98-457">Parameter Element (SSDL)</span></span>

<span data-ttu-id="23c98-458">Il **parametro** elemento schema store schema definition language (SSDL) è un figlio dell'elemento di funzione che specifica i parametri per una stored procedure nel database.</span><span class="sxs-lookup"><span data-stu-id="23c98-458">The **Parameter** element in store schema definition language (SSDL) is a child of the Function element that specifies parameters for a stored procedure in the database.</span></span>

<span data-ttu-id="23c98-459">Il **parametro** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="23c98-459">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="23c98-460">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="23c98-460">Documentation (zero or one)</span></span>
-   <span data-ttu-id="23c98-461">Elementi Annotation (zero o più)</span><span class="sxs-lookup"><span data-stu-id="23c98-461">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="23c98-462">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="23c98-462">Applicable Attributes</span></span>

<span data-ttu-id="23c98-463">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **parametro** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-463">The table below describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="23c98-464">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="23c98-464">Attribute Name</span></span> | <span data-ttu-id="23c98-465">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="23c98-465">Is Required</span></span> | <span data-ttu-id="23c98-466">Valore</span><span class="sxs-lookup"><span data-stu-id="23c98-466">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="23c98-467">**Name**</span><span class="sxs-lookup"><span data-stu-id="23c98-467">**Name**</span></span>       | <span data-ttu-id="23c98-468">Yes</span><span class="sxs-lookup"><span data-stu-id="23c98-468">Yes</span></span>         | <span data-ttu-id="23c98-469">Nome del parametro.</span><span class="sxs-lookup"><span data-stu-id="23c98-469">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="23c98-470">**Type**</span><span class="sxs-lookup"><span data-stu-id="23c98-470">**Type**</span></span>       | <span data-ttu-id="23c98-471">Yes</span><span class="sxs-lookup"><span data-stu-id="23c98-471">Yes</span></span>         | <span data-ttu-id="23c98-472">Tipo del parametro.</span><span class="sxs-lookup"><span data-stu-id="23c98-472">The parameter type.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="23c98-473">**Modalità**</span><span class="sxs-lookup"><span data-stu-id="23c98-473">**Mode**</span></span>       | <span data-ttu-id="23c98-474">No</span><span class="sxs-lookup"><span data-stu-id="23c98-474">No</span></span>          | <span data-ttu-id="23c98-475">**Nelle**, **Out**, o **InOut** a seconda che il parametro sia un input, output o parametro di input/output.</span><span class="sxs-lookup"><span data-stu-id="23c98-475">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="23c98-476">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="23c98-476">**MaxLength**</span></span>  | <span data-ttu-id="23c98-477">No</span><span class="sxs-lookup"><span data-stu-id="23c98-477">No</span></span>          | <span data-ttu-id="23c98-478">Lunghezza massima del parametro.</span><span class="sxs-lookup"><span data-stu-id="23c98-478">The maximum length of the parameter.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="23c98-479">**Precisione**</span><span class="sxs-lookup"><span data-stu-id="23c98-479">**Precision**</span></span>  | <span data-ttu-id="23c98-480">No</span><span class="sxs-lookup"><span data-stu-id="23c98-480">No</span></span>          | <span data-ttu-id="23c98-481">Precisione del parametro.</span><span class="sxs-lookup"><span data-stu-id="23c98-481">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="23c98-482">**Scala**</span><span class="sxs-lookup"><span data-stu-id="23c98-482">**Scale**</span></span>      | <span data-ttu-id="23c98-483">No</span><span class="sxs-lookup"><span data-stu-id="23c98-483">No</span></span>          | <span data-ttu-id="23c98-484">Scala del parametro.</span><span class="sxs-lookup"><span data-stu-id="23c98-484">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="23c98-485">**SRID**</span><span class="sxs-lookup"><span data-stu-id="23c98-485">**SRID**</span></span>       | <span data-ttu-id="23c98-486">No</span><span class="sxs-lookup"><span data-stu-id="23c98-486">No</span></span>          | <span data-ttu-id="23c98-487">Identificatore di riferimento spaziale del sistema.</span><span class="sxs-lookup"><span data-stu-id="23c98-487">Spatial System Reference Identifier.</span></span> <span data-ttu-id="23c98-488">Valido solo per i parametri di tipi spaziali.</span><span class="sxs-lookup"><span data-stu-id="23c98-488">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="23c98-489">Per altre informazioni, vedere [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="23c98-489">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

> [!NOTE]
> <span data-ttu-id="23c98-490">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **parametro** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-490">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="23c98-491">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="23c98-491">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="23c98-492">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="23c98-492">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="23c98-493">Esempio</span><span class="sxs-lookup"><span data-stu-id="23c98-493">Example</span></span>

<span data-ttu-id="23c98-494">L'esempio seguente mostra una **funzione** elemento che dispone di due **parametro** gli elementi che specificano i parametri di input:</span><span class="sxs-lookup"><span data-stu-id="23c98-494">The following example shows a **Function** element that has two **Parameter** elements that specify input parameters:</span></span>

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

## <a name="principal-element-ssdl"></a><span data-ttu-id="23c98-495">Elemento Principal (SSDL)</span><span class="sxs-lookup"><span data-stu-id="23c98-495">Principal Element (SSDL)</span></span>

<span data-ttu-id="23c98-496">Il **Principal** in schema store schema definition language (SSDL) è un elemento figlio all'elemento ReferentialConstraint che definisce l'entità finale del vincolo di chiave esterno (chiamato anche vincolo referenziale).</span><span class="sxs-lookup"><span data-stu-id="23c98-496">The **Principal** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the principal end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="23c98-497">Il **Principal** elemento specifica la colonna chiave primaria (o le colonne) in una tabella cui viene fatto riferimento da un'altra colonna (o colonne).</span><span class="sxs-lookup"><span data-stu-id="23c98-497">The **Principal** element specifies the primary key column (or columns) in a table that is referenced by another column (or columns).</span></span> <span data-ttu-id="23c98-498">**PropertyRef** elementi specificano quali colonne fanno riferimento.</span><span class="sxs-lookup"><span data-stu-id="23c98-498">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="23c98-499">Elemento Dependent specifica le colonne che fanno riferimento le colonne chiave primaria specificati nel **Principal** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-499">The Dependent element specifies columns that reference the primary key columns that are specified in the **Principal** element.</span></span>

<span data-ttu-id="23c98-500">Il **Principal** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="23c98-500">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="23c98-501">PropertyRef (una o più)</span><span class="sxs-lookup"><span data-stu-id="23c98-501">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="23c98-502">Elementi Annotation (zero o più)</span><span class="sxs-lookup"><span data-stu-id="23c98-502">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="23c98-503">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="23c98-503">Applicable Attributes</span></span>

<span data-ttu-id="23c98-504">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **Principal** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-504">The following table describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="23c98-505">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="23c98-505">Attribute Name</span></span> | <span data-ttu-id="23c98-506">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="23c98-506">Is Required</span></span> | <span data-ttu-id="23c98-507">Valore</span><span class="sxs-lookup"><span data-stu-id="23c98-507">Value</span></span>                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="23c98-508">**Ruolo**</span><span class="sxs-lookup"><span data-stu-id="23c98-508">**Role**</span></span>       | <span data-ttu-id="23c98-509">Yes</span><span class="sxs-lookup"><span data-stu-id="23c98-509">Yes</span></span>         | <span data-ttu-id="23c98-510">Lo stesso valore di **ruolo** (se utilizzato) dell'attributo dell'elemento End corrispondente; in caso contrario, il nome della tabella contenente la colonna di riferimento.</span><span class="sxs-lookup"><span data-stu-id="23c98-510">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referenced column.</span></span> |

> [!NOTE]
> <span data-ttu-id="23c98-511">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **Principal** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-511">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="23c98-512">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="23c98-512">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="23c98-513">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="23c98-513">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="23c98-514">Esempio</span><span class="sxs-lookup"><span data-stu-id="23c98-514">Example</span></span>

<span data-ttu-id="23c98-515">L'esempio seguente illustra un elemento di associazione che utilizza un **ReferentialConstraint** elemento per specificare le colonne che partecipano le **FK\_CustomerOrders** chiave esterna vincolo.</span><span class="sxs-lookup"><span data-stu-id="23c98-515">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="23c98-516">Il **dell'entità** elemento specifica il **CustomerId** colonna del **cliente** tabella come entità finale principale del vincolo.</span><span class="sxs-lookup"><span data-stu-id="23c98-516">The **Principal** element specifies the **CustomerId** column of the **Customer** table as the principal end of the constraint.</span></span>

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

## <a name="property-element-ssdl"></a><span data-ttu-id="23c98-517">Elemento Property (SSDL)</span><span class="sxs-lookup"><span data-stu-id="23c98-517">Property Element (SSDL)</span></span>

<span data-ttu-id="23c98-518">Il **proprietà** elemento schema store schema definition language (SSDL) rappresenta una colonna in una tabella nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="23c98-518">The **Property** element in store schema definition language (SSDL) represents a column in a table in the underlying database.</span></span> <span data-ttu-id="23c98-519">**Proprietà** elementi sono elementi figlio degli elementi EntityType, che rappresentano le righe in una tabella.</span><span class="sxs-lookup"><span data-stu-id="23c98-519">**Property** elements are children of EntityType elements, which represent rows in a table.</span></span> <span data-ttu-id="23c98-520">Ciascuna **proprietà** elemento definito in un **EntityType** elemento rappresenta una colonna.</span><span class="sxs-lookup"><span data-stu-id="23c98-520">Each **Property** element defined on an **EntityType** element represents a column.</span></span>

<span data-ttu-id="23c98-521">Oggetto **proprietà** elemento non può avere elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="23c98-521">A **Property** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="23c98-522">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="23c98-522">Applicable Attributes</span></span>

<span data-ttu-id="23c98-523">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **proprietà** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-523">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="23c98-524">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="23c98-524">Attribute Name</span></span>            | <span data-ttu-id="23c98-525">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="23c98-525">Is Required</span></span> | <span data-ttu-id="23c98-526">Valore</span><span class="sxs-lookup"><span data-stu-id="23c98-526">Value</span></span>                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="23c98-527">**Name**</span><span class="sxs-lookup"><span data-stu-id="23c98-527">**Name**</span></span>                  | <span data-ttu-id="23c98-528">Yes</span><span class="sxs-lookup"><span data-stu-id="23c98-528">Yes</span></span>         | <span data-ttu-id="23c98-529">Nome della colonna corrispondente.</span><span class="sxs-lookup"><span data-stu-id="23c98-529">The name of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="23c98-530">**Type**</span><span class="sxs-lookup"><span data-stu-id="23c98-530">**Type**</span></span>                  | <span data-ttu-id="23c98-531">Yes</span><span class="sxs-lookup"><span data-stu-id="23c98-531">Yes</span></span>         | <span data-ttu-id="23c98-532">Tipo della colonna corrispondente.</span><span class="sxs-lookup"><span data-stu-id="23c98-532">The type of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="23c98-533">**Ammette valori null**</span><span class="sxs-lookup"><span data-stu-id="23c98-533">**Nullable**</span></span>              | <span data-ttu-id="23c98-534">No</span><span class="sxs-lookup"><span data-stu-id="23c98-534">No</span></span>          | <span data-ttu-id="23c98-535">**True** (valore predefinito) o **False** a seconda del fatto che la colonna corrispondente può avere un valore null.</span><span class="sxs-lookup"><span data-stu-id="23c98-535">**True** (the default value) or **False** depending on whether the corresponding column can have a null value.</span></span>                                                                                                                  |
| <span data-ttu-id="23c98-536">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="23c98-536">**DefaultValue**</span></span>          | <span data-ttu-id="23c98-537">No</span><span class="sxs-lookup"><span data-stu-id="23c98-537">No</span></span>          | <span data-ttu-id="23c98-538">Valore predefinito della colonna corrispondente.</span><span class="sxs-lookup"><span data-stu-id="23c98-538">The default value of the corresponding column.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="23c98-539">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="23c98-539">**MaxLength**</span></span>             | <span data-ttu-id="23c98-540">No</span><span class="sxs-lookup"><span data-stu-id="23c98-540">No</span></span>          | <span data-ttu-id="23c98-541">Lunghezza massima della colonna corrispondente.</span><span class="sxs-lookup"><span data-stu-id="23c98-541">The maximum length of the corresponding column.</span></span>                                                                                                                                                                                 |
| <span data-ttu-id="23c98-542">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="23c98-542">**FixedLength**</span></span>           | <span data-ttu-id="23c98-543">No</span><span class="sxs-lookup"><span data-stu-id="23c98-543">No</span></span>          | <span data-ttu-id="23c98-544">**True** oppure **False** a seconda se il valore della colonna corrispondente essere archiviato come una stringa di lunghezza fissa.</span><span class="sxs-lookup"><span data-stu-id="23c98-544">**True** or **False** depending on whether the corresponding column value will be stored as a fixed length string.</span></span>                                                                                                              |
| <span data-ttu-id="23c98-545">**Precisione**</span><span class="sxs-lookup"><span data-stu-id="23c98-545">**Precision**</span></span>             | <span data-ttu-id="23c98-546">No</span><span class="sxs-lookup"><span data-stu-id="23c98-546">No</span></span>          | <span data-ttu-id="23c98-547">Precisione della colonna corrispondente.</span><span class="sxs-lookup"><span data-stu-id="23c98-547">The precision of the corresponding column.</span></span>                                                                                                                                                                                      |
| <span data-ttu-id="23c98-548">**Scala**</span><span class="sxs-lookup"><span data-stu-id="23c98-548">**Scale**</span></span>                 | <span data-ttu-id="23c98-549">No</span><span class="sxs-lookup"><span data-stu-id="23c98-549">No</span></span>          | <span data-ttu-id="23c98-550">Scala della colonna corrispondente.</span><span class="sxs-lookup"><span data-stu-id="23c98-550">The scale of the corresponding column.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="23c98-551">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="23c98-551">**Unicode**</span></span>               | <span data-ttu-id="23c98-552">No</span><span class="sxs-lookup"><span data-stu-id="23c98-552">No</span></span>          | <span data-ttu-id="23c98-553">**True** oppure **False** a seconda se il valore della colonna corrispondente essere archiviato come stringa Unicode.</span><span class="sxs-lookup"><span data-stu-id="23c98-553">**True** or **False** depending on whether the corresponding column value will be stored as a Unicode string.</span></span>                                                                                                                   |
| <span data-ttu-id="23c98-554">**regole di confronto**</span><span class="sxs-lookup"><span data-stu-id="23c98-554">**Collation**</span></span>             | <span data-ttu-id="23c98-555">No</span><span class="sxs-lookup"><span data-stu-id="23c98-555">No</span></span>          | <span data-ttu-id="23c98-556">Stringa che specifica la sequenza di confronto da utilizzare nell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="23c98-556">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="23c98-557">**SRID**</span><span class="sxs-lookup"><span data-stu-id="23c98-557">**SRID**</span></span>                  | <span data-ttu-id="23c98-558">No</span><span class="sxs-lookup"><span data-stu-id="23c98-558">No</span></span>          | <span data-ttu-id="23c98-559">Identificatore di riferimento spaziale del sistema.</span><span class="sxs-lookup"><span data-stu-id="23c98-559">Spatial System Reference Identifier.</span></span> <span data-ttu-id="23c98-560">Valido solo per le proprietà di tipi spaziali.</span><span class="sxs-lookup"><span data-stu-id="23c98-560">Valid only for properties of spatial types.</span></span> <span data-ttu-id="23c98-561">Per altre informazioni, vedere [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="23c98-561">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="23c98-562">**StoreGeneratedPattern**</span><span class="sxs-lookup"><span data-stu-id="23c98-562">**StoreGeneratedPattern**</span></span> | <span data-ttu-id="23c98-563">No</span><span class="sxs-lookup"><span data-stu-id="23c98-563">No</span></span>          | <span data-ttu-id="23c98-564">**None**, **Identity** (se il valore della colonna corrispondente è un'identità generata nel database) oppure **calcolata** (se il valore della colonna corrispondente viene calcolato nel database).</span><span class="sxs-lookup"><span data-stu-id="23c98-564">**None**, **Identity** (if the corresponding column value is an identity that is generated in the database), or **Computed** (if the corresponding column value is computed in the database).</span></span> <span data-ttu-id="23c98-565">Non valido per le proprietà di tipo di riga.</span><span class="sxs-lookup"><span data-stu-id="23c98-565">Not Valid for RowType properties.</span></span> |

> [!NOTE]
> <span data-ttu-id="23c98-566">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **proprietà** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-566">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="23c98-567">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="23c98-567">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="23c98-568">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="23c98-568">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="23c98-569">Esempio</span><span class="sxs-lookup"><span data-stu-id="23c98-569">Example</span></span>

<span data-ttu-id="23c98-570">L'esempio seguente mostra un' **EntityType** elemento con l'elemento figlio due **proprietà** elementi:</span><span class="sxs-lookup"><span data-stu-id="23c98-570">The following example shows an **EntityType** element with two child **Property** elements:</span></span>

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

## <a name="propertyref-element-ssdl"></a><span data-ttu-id="23c98-571">Elemento PropertyRef (SSDL)</span><span class="sxs-lookup"><span data-stu-id="23c98-571">PropertyRef Element (SSDL)</span></span>

<span data-ttu-id="23c98-572">Il **PropertyRef** elemento schema store schema definition language (SSDL) fa riferimento a una proprietà definita in un elemento EntityType per indicare che la proprietà eseguirà uno dei seguenti ruoli:</span><span class="sxs-lookup"><span data-stu-id="23c98-572">The **PropertyRef** element in store schema definition language (SSDL) references a property defined on an EntityType element to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="23c98-573">Far parte della chiave primaria della tabella che la **EntityType** rappresenta.</span><span class="sxs-lookup"><span data-stu-id="23c98-573">Be part of the primary key of the table that the **EntityType** represents.</span></span> <span data-ttu-id="23c98-574">Uno o più **PropertyRef** elementi possono essere utilizzati per definire una chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="23c98-574">One or more **PropertyRef** elements can be used to define a primary key.</span></span> <span data-ttu-id="23c98-575">Per altre informazioni, vedere l'elemento Key.</span><span class="sxs-lookup"><span data-stu-id="23c98-575">For more information, see Key element.</span></span>
-   <span data-ttu-id="23c98-576">Rappresenterà l'entità finale dipendente o principale di un vincolo referenziale.</span><span class="sxs-lookup"><span data-stu-id="23c98-576">Be the dependent or principal end of a referential constraint.</span></span> <span data-ttu-id="23c98-577">Per altre informazioni, vedere ReferentialConstraint (elemento).</span><span class="sxs-lookup"><span data-stu-id="23c98-577">For more information, see ReferentialConstraint element.</span></span>

<span data-ttu-id="23c98-578">Il **PropertyRef** elemento può avere solo elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="23c98-578">The **PropertyRef** element can only have the following child elements:</span></span>

-   <span data-ttu-id="23c98-579">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="23c98-579">Documentation (zero or one)</span></span>
-   <span data-ttu-id="23c98-580">Elementi Annotation</span><span class="sxs-lookup"><span data-stu-id="23c98-580">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="23c98-581">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="23c98-581">Applicable Attributes</span></span>

<span data-ttu-id="23c98-582">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **PropertyRef** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-582">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="23c98-583">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="23c98-583">Attribute Name</span></span> | <span data-ttu-id="23c98-584">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="23c98-584">Is Required</span></span> | <span data-ttu-id="23c98-585">Valore</span><span class="sxs-lookup"><span data-stu-id="23c98-585">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="23c98-586">**Name**</span><span class="sxs-lookup"><span data-stu-id="23c98-586">**Name**</span></span>       | <span data-ttu-id="23c98-587">Yes</span><span class="sxs-lookup"><span data-stu-id="23c98-587">Yes</span></span>         | <span data-ttu-id="23c98-588">Nome della proprietà alla quale viene fatto riferimento.</span><span class="sxs-lookup"><span data-stu-id="23c98-588">The name of the referenced property.</span></span> |

> [!NOTE]
> <span data-ttu-id="23c98-589">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **PropertyRef** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-589">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="23c98-590">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="23c98-590">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="23c98-591">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="23c98-591">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="23c98-592">Esempio</span><span class="sxs-lookup"><span data-stu-id="23c98-592">Example</span></span>

<span data-ttu-id="23c98-593">L'esempio seguente mostra una **PropertyRef** elemento usato per definire una chiave primaria facendo riferimento a una proprietà definita in un **EntityType** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-593">The following example shows a **PropertyRef** element used to define a primary key by referencing a property that is defined on an **EntityType** element.</span></span>

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

## <a name="referentialconstraint-element-ssdl"></a><span data-ttu-id="23c98-594">Elemento ReferentialConstraint (SSDL)</span><span class="sxs-lookup"><span data-stu-id="23c98-594">ReferentialConstraint Element (SSDL)</span></span>

<span data-ttu-id="23c98-595">Il **ReferentialConstraint** elemento schema store schema definition language (SSDL) rappresenta un vincolo di chiave esterna (chiamato anche vincolo referenziale) nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="23c98-595">The **ReferentialConstraint** element in store schema definition language (SSDL) represents a foreign key constraint (also called a referential integrity constraint) in the underlying database.</span></span> <span data-ttu-id="23c98-596">Le entità finali principale e dipendente del vincolo vengono specificate dagli elementi figlio principale e dipendente, rispettivamente.</span><span class="sxs-lookup"><span data-stu-id="23c98-596">The principal and dependent ends of the constraint are specified by the Principal and Dependent child elements, respectively.</span></span> <span data-ttu-id="23c98-597">Colonne che partecipano all'estremità principale e dipendente vengono fatto riferimento con gli elementi PropertyRef.</span><span class="sxs-lookup"><span data-stu-id="23c98-597">Columns that participate in the principal and dependent ends are referenced with PropertyRef elements.</span></span>

<span data-ttu-id="23c98-598">Il **ReferentialConstraint** è un elemento figlio facoltativo dell'elemento Association.</span><span class="sxs-lookup"><span data-stu-id="23c98-598">The **ReferentialConstraint** element is an optional child element of the Association element.</span></span> <span data-ttu-id="23c98-599">Se un **ReferentialConstraint** elemento non viene usato per eseguire il mapping il vincolo foreign key specificato nella **Association** elemento, un AssociationSetMapping elemento deve essere usato per eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="23c98-599">If a **ReferentialConstraint** element is not used to map the foreign key constraint that is specified in the **Association** element, an AssociationSetMapping element must be used to do this.</span></span>

<span data-ttu-id="23c98-600">Il **ReferentialConstraint** elemento può avere elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="23c98-600">The **ReferentialConstraint** element can have the following child elements:</span></span>

-   <span data-ttu-id="23c98-601">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="23c98-601">Documentation (zero or one)</span></span>
-   <span data-ttu-id="23c98-602">Entità (esattamente un elemento)</span><span class="sxs-lookup"><span data-stu-id="23c98-602">Principal (exactly one)</span></span>
-   <span data-ttu-id="23c98-603">Dipendente (esattamente un elemento)</span><span class="sxs-lookup"><span data-stu-id="23c98-603">Dependent (exactly one)</span></span>
-   <span data-ttu-id="23c98-604">Elementi Annotation (zero o più)</span><span class="sxs-lookup"><span data-stu-id="23c98-604">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="23c98-605">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="23c98-605">Applicable Attributes</span></span>

<span data-ttu-id="23c98-606">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **ReferentialConstraint** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-606">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferentialConstraint** element.</span></span> <span data-ttu-id="23c98-607">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="23c98-607">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="23c98-608">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="23c98-608">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="23c98-609">Esempio</span><span class="sxs-lookup"><span data-stu-id="23c98-609">Example</span></span>

<span data-ttu-id="23c98-610">L'esempio seguente mostra un' **Association** elemento che utilizza un **ReferentialConstraint** elemento per specificare le colonne che fanno parte del **FK\_CustomerOrders**  vincolo di chiave esterna:</span><span class="sxs-lookup"><span data-stu-id="23c98-610">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="returntype-element-ssdl"></a><span data-ttu-id="23c98-611">Elemento ReturnType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="23c98-611">ReturnType Element (SSDL)</span></span>

<span data-ttu-id="23c98-612">Il **ReturnType** elemento di schema store schema definition language (SSDL) specifica il tipo restituito per una funzione che viene definito in un **funzione** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-612">The **ReturnType** element in store schema definition language (SSDL) specifies the return type for a function that is defined in a **Function** element.</span></span> <span data-ttu-id="23c98-613">Una funzione di tipo restituito può essere specificato anche con un **ReturnType** attributo.</span><span class="sxs-lookup"><span data-stu-id="23c98-613">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="23c98-614">Il tipo restituito di una funzione viene specificato con il **tipo** attributo o il **ReturnType** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-614">The return type of a function is specified with the **Type** attribute or the **ReturnType** element.</span></span>

<span data-ttu-id="23c98-615">Il **ReturnType** elemento può avere elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="23c98-615">The **ReturnType** element can have the following child elements:</span></span>

- <span data-ttu-id="23c98-616">Elemento CollectionType (uno)</span><span class="sxs-lookup"><span data-stu-id="23c98-616">CollectionType (one)</span></span>  

> [!NOTE]
> <span data-ttu-id="23c98-617">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **ReturnType** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-617">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** element.</span></span> <span data-ttu-id="23c98-618">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="23c98-618">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="23c98-619">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="23c98-619">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="23c98-620">Esempio</span><span class="sxs-lookup"><span data-stu-id="23c98-620">Example</span></span>

<span data-ttu-id="23c98-621">L'esempio seguente usa un' **funzione** che restituisce una raccolta di righe.</span><span class="sxs-lookup"><span data-stu-id="23c98-621">The following example uses a **Function** that returns a collection of rows.</span></span>

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


## <a name="rowtype-element-ssdl"></a><span data-ttu-id="23c98-622">Elemento RowType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="23c98-622">RowType Element (SSDL)</span></span>

<span data-ttu-id="23c98-623">Oggetto **RowType** elemento schema store schema definition language (SSDL) definisce una struttura senza nome come reso tipo per una funzione definita nell'archivio.</span><span class="sxs-lookup"><span data-stu-id="23c98-623">A **RowType** element in store schema definition language (SSDL) defines an unnamed structure as a return type for a function defined in the store.</span></span>

<span data-ttu-id="23c98-624">Oggetto **RowType** è l'elemento figlio di **CollectionType** elemento:</span><span class="sxs-lookup"><span data-stu-id="23c98-624">A **RowType** element is the child element of **CollectionType** element:</span></span>

<span data-ttu-id="23c98-625">Oggetto **RowType** elemento può avere elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="23c98-625">A **RowType** element can have the following child elements:</span></span>

- <span data-ttu-id="23c98-626">Proprietà (una o più)</span><span class="sxs-lookup"><span data-stu-id="23c98-626">Property (one or more)</span></span>  

### <a name="example"></a><span data-ttu-id="23c98-627">Esempio</span><span class="sxs-lookup"><span data-stu-id="23c98-627">Example</span></span>

<span data-ttu-id="23c98-628">Nell'esempio seguente viene illustrata una funzione di archivio che usa un' **CollectionType** elemento per specificare che la funzione restituisce una raccolta di righe (come specificato nella **RowType** elemento).</span><span class="sxs-lookup"><span data-stu-id="23c98-628">The following example shows a store function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>


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

## <a name="schema-element-ssdl"></a><span data-ttu-id="23c98-629">Elemento Schema (SSDL)</span><span class="sxs-lookup"><span data-stu-id="23c98-629">Schema Element (SSDL)</span></span>

<span data-ttu-id="23c98-630">Il **Schema** elemento schema store schema definition language (SSDL) è l'elemento radice di una definizione di modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="23c98-630">The **Schema** element in store schema definition language (SSDL) is the root element of a storage model definition.</span></span> <span data-ttu-id="23c98-631">Contiene definizioni per gli oggetti, le funzioni e i contenitori che costituiscono un modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="23c98-631">It contains definitions for the objects, functions, and containers that make up a storage model.</span></span>

<span data-ttu-id="23c98-632">Il **Schema** elemento può contenere zero o più degli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="23c98-632">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="23c98-633">Associazione</span><span class="sxs-lookup"><span data-stu-id="23c98-633">Association</span></span>
-   <span data-ttu-id="23c98-634">EntityType</span><span class="sxs-lookup"><span data-stu-id="23c98-634">EntityType</span></span>
-   <span data-ttu-id="23c98-635">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="23c98-635">EntityContainer</span></span>
-   <span data-ttu-id="23c98-636">Funzione</span><span class="sxs-lookup"><span data-stu-id="23c98-636">Function</span></span>

<span data-ttu-id="23c98-637">Il **Schema** elemento utilizza le **Namespace** attributo per definire lo spazio dei nomi per gli oggetti di tipo e associazione di entità in un modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="23c98-637">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type and association objects in a storage model.</span></span> <span data-ttu-id="23c98-638">All'interno di uno spazio dei nomi due oggetti non possono avere lo stesso nome.</span><span class="sxs-lookup"><span data-stu-id="23c98-638">Within a namespace, no two objects can have the same name.</span></span>

<span data-ttu-id="23c98-639">Uno spazio dei nomi del modello di archiviazione è diverso dallo spazio dei nomi XML del **Schema** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-639">A storage model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="23c98-640">Uno spazio dei nomi del modello di archiviazione (come definito per il **Namespace** attributi) è un contenitore logico per tipi di entità e tipi di associazione.</span><span class="sxs-lookup"><span data-stu-id="23c98-640">A storage model namespace (as defined by the **Namespace** attribute) is a logical container for entity types and association types.</span></span> <span data-ttu-id="23c98-641">Lo spazio dei nomi XML (indicato dal **xmlns** attributo) di un **Schema** elemento è lo spazio dei nomi predefinito per gli elementi figlio e attributi del **Schema** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-641">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="23c98-642">Spazi dei nomi XML nel formato http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (dove YYYY e MM rappresentano un anno e mese rispettivamente) sono riservati a SSDL.</span><span class="sxs-lookup"><span data-stu-id="23c98-642">XML namespaces of the form http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (where YYYY and MM represent a year and month respectively) are reserved for SSDL.</span></span> <span data-ttu-id="23c98-643">Gli elementi e gli attributi personalizzati non possono essere in spazi dei nomi che hanno questo form.</span><span class="sxs-lookup"><span data-stu-id="23c98-643">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="23c98-644">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="23c98-644">Applicable Attributes</span></span>

<span data-ttu-id="23c98-645">Nella tabella seguente vengono descritti gli attributi possono essere applicati per il **Schema** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-645">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="23c98-646">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="23c98-646">Attribute Name</span></span>            | <span data-ttu-id="23c98-647">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="23c98-647">Is Required</span></span> | <span data-ttu-id="23c98-648">Valore</span><span class="sxs-lookup"><span data-stu-id="23c98-648">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="23c98-649">**Spazio dei nomi**</span><span class="sxs-lookup"><span data-stu-id="23c98-649">**Namespace**</span></span>             | <span data-ttu-id="23c98-650">Yes</span><span class="sxs-lookup"><span data-stu-id="23c98-650">Yes</span></span>         | <span data-ttu-id="23c98-651">Spazio dei nomi del modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="23c98-651">The namespace of the storage model.</span></span> <span data-ttu-id="23c98-652">Il valore della **Namespace** viene usato in modo da formare il nome completo di un tipo.</span><span class="sxs-lookup"><span data-stu-id="23c98-652">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="23c98-653">Ad esempio, se un **EntityType** denominata *cliente* è lo spazio dei nomi examplemodel. Store, quindi specificare il nome completo del **EntityType** è Examplemodel.</span><span class="sxs-lookup"><span data-stu-id="23c98-653">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace, then the fully qualified name of the **EntityType** is ExampleModel.Store.Customer.</span></span> <br/> <span data-ttu-id="23c98-654">Le stringhe seguenti non possono essere usate come valore per il **Namespace** attributo: **System**, **temporaneo**, oppure **Edm**.</span><span class="sxs-lookup"><span data-stu-id="23c98-654">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="23c98-655">Il valore per il **Namespace** attributo non può essere identico al valore per il **Namespace** attributo nell'elemento Schema CSDL.</span><span class="sxs-lookup"><span data-stu-id="23c98-655">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the CSDL Schema element.</span></span> |
| <span data-ttu-id="23c98-656">**Alias**</span><span class="sxs-lookup"><span data-stu-id="23c98-656">**Alias**</span></span>                 | <span data-ttu-id="23c98-657">No</span><span class="sxs-lookup"><span data-stu-id="23c98-657">No</span></span>          | <span data-ttu-id="23c98-658">Identificatore utilizzato al posto del nome dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="23c98-658">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="23c98-659">Ad esempio, se un **EntityType** denominata *Customer* è nello spazio dei nomi examplemodel. Store e il valore del **Alias** attributo è *StorageModel*, è possibile utilizzare storagemodel. Customer come nome completo del **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="23c98-659">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace and the value of the **Alias** attribute is *StorageModel*, then you can use StorageModel.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="23c98-660">**Provider**</span><span class="sxs-lookup"><span data-stu-id="23c98-660">**Provider**</span></span>              | <span data-ttu-id="23c98-661">Yes</span><span class="sxs-lookup"><span data-stu-id="23c98-661">Yes</span></span>         | <span data-ttu-id="23c98-662">Provider di dati.</span><span class="sxs-lookup"><span data-stu-id="23c98-662">The data provider.</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="23c98-663">**ProviderManifestToken**</span><span class="sxs-lookup"><span data-stu-id="23c98-663">**ProviderManifestToken**</span></span> | <span data-ttu-id="23c98-664">Yes</span><span class="sxs-lookup"><span data-stu-id="23c98-664">Yes</span></span>         | <span data-ttu-id="23c98-665">Token che indica al provider quale manifesto del provider restituire.</span><span class="sxs-lookup"><span data-stu-id="23c98-665">A token that indicates to the provider which provider manifest to return.</span></span> <span data-ttu-id="23c98-666">Non è definito alcun formato per il token.</span><span class="sxs-lookup"><span data-stu-id="23c98-666">No format for the token is defined.</span></span> <span data-ttu-id="23c98-667">I valori per il token sono definiti dal provider.</span><span class="sxs-lookup"><span data-stu-id="23c98-667">Values for the token are defined by the provider.</span></span> <span data-ttu-id="23c98-668">Per informazioni sui token del manifesto del provider SQL Server, vedere SqlClient per Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="23c98-668">For information about SQL Server provider manifest tokens, see SqlClient for Entity Framework.</span></span>                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a><span data-ttu-id="23c98-669">Esempio</span><span class="sxs-lookup"><span data-stu-id="23c98-669">Example</span></span>

<span data-ttu-id="23c98-670">L'esempio seguente mostra una **Schema** elemento che contiene un' **EntityContainer** elemento, due **EntityType** elementi e uno **associazione** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-670">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

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

## <a name="annotation-attributes"></a><span data-ttu-id="23c98-671">Attributi di annotazione</span><span class="sxs-lookup"><span data-stu-id="23c98-671">Annotation Attributes</span></span>

<span data-ttu-id="23c98-672">Gli attributi di annotazione in Store Schema Definition Language (SSDL) sono attributi XML personalizzati nel modello di archiviazione che forniscono metadati aggiuntivi sugli elementi del modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="23c98-672">Annotation attributes in store schema definition language (SSDL) are custom XML attributes in the storage model that provide extra metadata about the elements in the storage model.</span></span> <span data-ttu-id="23c98-673">Oltre ad avere una struttura XML valida, agli attributi di annotazione si applicano i vincoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="23c98-673">In addition to having valid XML structure, the following constraints apply to annotation attributes:</span></span>

-   <span data-ttu-id="23c98-674">Gli attributi di annotazione non devono trovarsi in spazi dei nomi XML riservati a SSDL.</span><span class="sxs-lookup"><span data-stu-id="23c98-674">Annotation attributes must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="23c98-675">I nomi completi di due attributi di annotazione non devono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="23c98-675">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="23c98-676">È possibile applicare più attributi di annotazione a un determinato elemento SSDL.</span><span class="sxs-lookup"><span data-stu-id="23c98-676">More than one annotation attribute may be applied to a given SSDL element.</span></span> <span data-ttu-id="23c98-677">Metadati contenuti negli elementi di annotazione sono accessibili in fase di esecuzione usando le classi nello spazio dei nomi System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="23c98-677">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="23c98-678">Esempio</span><span class="sxs-lookup"><span data-stu-id="23c98-678">Example</span></span>

<span data-ttu-id="23c98-679">L'esempio seguente illustra un elemento EntityType che presenta un attributo di annotazione applicato per il **OrderId** proprietà.</span><span class="sxs-lookup"><span data-stu-id="23c98-679">The following example shows an EntityType element that has an annotation attribute applied to the **OrderId** property.</span></span> <span data-ttu-id="23c98-680">L'esempio illustra anche un elemento di annotazione aggiunto per il **EntityType** elemento.</span><span class="sxs-lookup"><span data-stu-id="23c98-680">The example also show an annotation element added to the **EntityType** element.</span></span>

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

## <a name="annotation-elements-ssdl"></a><span data-ttu-id="23c98-681">Elementi Annotation (SSDL)</span><span class="sxs-lookup"><span data-stu-id="23c98-681">Annotation Elements (SSDL)</span></span>

<span data-ttu-id="23c98-682">Gli elementi Annotation in SSDL (Store Schema Definition Language) sono elementi XML personalizzati nel modello di archiviazione che forniscono metadati aggiuntivi sul modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="23c98-682">Annotation elements in store schema definition language (SSDL) are custom XML elements in the storage model that provide extra metadata about the storage model.</span></span> <span data-ttu-id="23c98-683">Oltre ad avere una struttura XML valida, agli elementi Annotation si applicano i vincoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="23c98-683">In addition to having valid XML structure, the following constraints apply to annotation elements:</span></span>

-   <span data-ttu-id="23c98-684">Gli elementi Annotation non devono trovarsi in spazi dei nomi XML riservati a SSDL.</span><span class="sxs-lookup"><span data-stu-id="23c98-684">Annotation elements must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="23c98-685">I nomi completi di due elementi Annotation non devono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="23c98-685">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="23c98-686">Gli elementi Annotation devono apparire dopo tutti gli altri elementi figlio di un dato elemento SSDL.</span><span class="sxs-lookup"><span data-stu-id="23c98-686">Annotation elements must appear after all other child elements of a given SSDL element.</span></span>

<span data-ttu-id="23c98-687">Più elementi Annotation possono essere figli di un dato elemento SSDL.</span><span class="sxs-lookup"><span data-stu-id="23c98-687">More than one annotation element may be a child of a given SSDL element.</span></span> <span data-ttu-id="23c98-688">A partire da .NET Framework versione 4, i metadati contenuti in elementi annotation sono accessibile in fase di esecuzione usando le classi nello spazio dei nomi System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="23c98-688">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="23c98-689">Esempio</span><span class="sxs-lookup"><span data-stu-id="23c98-689">Example</span></span>

<span data-ttu-id="23c98-690">L'esempio seguente illustra un elemento EntityType contenente un elemento annotation (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="23c98-690">The following example shows an EntityType element that has an annotation element (**CustomElement**).</span></span> <span data-ttu-id="23c98-691">L'esempio mostra anche un attributo di annotazione applicato per il **OrderId** proprietà.</span><span class="sxs-lookup"><span data-stu-id="23c98-691">The example also shows an annotation attribute applied to the **OrderId** property.</span></span>

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

## <a name="facets-ssdl"></a><span data-ttu-id="23c98-692">Facet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="23c98-692">Facets (SSDL)</span></span>

<span data-ttu-id="23c98-693">I facet in schema store schema definition language (SSDL) rappresentano vincoli sui tipi di colonna sono specificati negli elementi proprietà.</span><span class="sxs-lookup"><span data-stu-id="23c98-693">Facets in store schema definition language (SSDL) represent constraints on column types that are specified in Property elements.</span></span> <span data-ttu-id="23c98-694">I facet vengono implementati come attributi XML sul **proprietà** elementi.</span><span class="sxs-lookup"><span data-stu-id="23c98-694">Facets are implemented as XML attributes on **Property** elements.</span></span>

<span data-ttu-id="23c98-695">Nella tabella seguente vengono descritti i facet supportati in SSDL:</span><span class="sxs-lookup"><span data-stu-id="23c98-695">The following table describes the facets that are supported in SSDL:</span></span>

| <span data-ttu-id="23c98-696">Facet</span><span class="sxs-lookup"><span data-stu-id="23c98-696">Facet</span></span>           | <span data-ttu-id="23c98-697">Descrizione</span><span class="sxs-lookup"><span data-stu-id="23c98-697">Description</span></span>                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="23c98-698">**regole di confronto**</span><span class="sxs-lookup"><span data-stu-id="23c98-698">**Collation**</span></span>   | <span data-ttu-id="23c98-699">Specifica la sequenza di ordinamento da usare quando si eseguono operazioni di confronto e di ordinamento su valori della proprietà.</span><span class="sxs-lookup"><span data-stu-id="23c98-699">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                             |
| <span data-ttu-id="23c98-700">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="23c98-700">**FixedLength**</span></span> | <span data-ttu-id="23c98-701">Specifica se la lunghezza del valore della colonna può variare.</span><span class="sxs-lookup"><span data-stu-id="23c98-701">Specifies whether the length of the column value can vary.</span></span>                                                                                                                                                                                                  |
| <span data-ttu-id="23c98-702">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="23c98-702">**MaxLength**</span></span>   | <span data-ttu-id="23c98-703">Specifica la lunghezza massima del valore della colonna.</span><span class="sxs-lookup"><span data-stu-id="23c98-703">Specifies the maximum length of the column value.</span></span>                                                                                                                                                                                                           |
| <span data-ttu-id="23c98-704">**Precisione**</span><span class="sxs-lookup"><span data-stu-id="23c98-704">**Precision**</span></span>   | <span data-ttu-id="23c98-705">Per le proprietà di tipo **decimale**, specifica il numero di cifre può avere un valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="23c98-705">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="23c98-706">Per le proprietà di tipo **tempo**, **DateTime**, e **DateTimeOffset**, specifica il numero di cifre per la parte frazionaria di secondi del valore della colonna.</span><span class="sxs-lookup"><span data-stu-id="23c98-706">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the column value.</span></span> |
| <span data-ttu-id="23c98-707">**Scala**</span><span class="sxs-lookup"><span data-stu-id="23c98-707">**Scale**</span></span>       | <span data-ttu-id="23c98-708">Specifica il numero di cifre a destra del separatore decimale per il valore della colonna.</span><span class="sxs-lookup"><span data-stu-id="23c98-708">Specifies the number of digits to the right of the decimal point for the column value.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="23c98-709">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="23c98-709">**Unicode**</span></span>     | <span data-ttu-id="23c98-710">Indica se il valore della colonna viene archiviato come Unicode.</span><span class="sxs-lookup"><span data-stu-id="23c98-710">Indicates whether the column value is stored as Unicode.</span></span>                                                                                                                                                                                                    |
