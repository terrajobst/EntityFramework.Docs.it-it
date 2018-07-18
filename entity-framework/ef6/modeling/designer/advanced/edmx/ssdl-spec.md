---
title: Specifica di SSDL - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
caps.latest.revision: 3
ms.openlocfilehash: a9977c80d9a9401afdcad2284a705bcb28790fb8
ms.sourcegitcommit: 9ae4473425c5e76337c9d032b0e5dbfedf1fcf57
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121449"
---
# <a name="ssdl-specification"></a><span data-ttu-id="74111-102">Specifica SSDL</span><span class="sxs-lookup"><span data-stu-id="74111-102">SSDL Specification</span></span>
<span data-ttu-id="74111-103">Store Schema Definition Language (SSDL) è un linguaggio basato su XML che descrive il modello di archiviazione di un'applicazione Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="74111-103">Store schema definition language (SSDL) is an XML-based language that describes the storage model of an Entity Framework application.</span></span>

<span data-ttu-id="74111-104">In un'applicazione Entity Framework, i metadati del modello di archiviazione viene caricato da un file con estensione ssdl, scritto in SSDL, in un'istanza del System.Data.Metadata.Edm.StoreItemCollection e sia accessibili tramite i metodi di Classe System.Data.Metadata.Edm.MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="74111-104">In an Entity Framework application, storage model metadata is loaded from a .ssdl file (written in SSDL) into an instance of the System.Data.Metadata.Edm.StoreItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="74111-105">Entity Framework Usa i metadati del modello di archiviazione per tradurre le query sul modello concettuale in comandi specifici dell'archivio.</span><span class="sxs-lookup"><span data-stu-id="74111-105">Entity Framework uses storage model metadata to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="74111-106">Entity Framework Designer (Entity Framework Designer) archivia le informazioni sul modello di archiviazione in un file con estensione edmx in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="74111-106">The Entity Framework Designer (EF Designer) stores storage model information in an .edmx file at design time.</span></span> <span data-ttu-id="74111-107">In fase di compilazione Entity Designer utilizza le informazioni in un file con estensione edmx per creare il file con estensione SSDL che serve da Entity Framework in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="74111-107">At build time the Entity Designer uses information in an .edmx file to create the .ssdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="74111-108">Le versioni di SSDL si differenziano tra loro per gli spazi dei nomi XML.</span><span class="sxs-lookup"><span data-stu-id="74111-108">Versions of SSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="74111-109">Versione SSDL</span><span class="sxs-lookup"><span data-stu-id="74111-109">SSDL Version</span></span> | <span data-ttu-id="74111-110">Namespace XML</span><span class="sxs-lookup"><span data-stu-id="74111-110">XML Namespace</span></span>                                     |
|:-------------|:--------------------------------------------------|
| <span data-ttu-id="74111-111">SSDL v1</span><span class="sxs-lookup"><span data-stu-id="74111-111">SSDL v1</span></span>      | http://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| <span data-ttu-id="74111-112">SSDL v2</span><span class="sxs-lookup"><span data-stu-id="74111-112">SSDL v2</span></span>      | http://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| <span data-ttu-id="74111-113">SSDL v3</span><span class="sxs-lookup"><span data-stu-id="74111-113">SSDL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a><span data-ttu-id="74111-114">Elemento Association (SSDL)</span><span class="sxs-lookup"><span data-stu-id="74111-114">Association Element (SSDL)</span></span>

<span data-ttu-id="74111-115">Un' **Association** elemento schema store schema definition language (SSDL) consente di specificare le colonne di tabella che fanno parte di un vincolo di chiave esterna nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="74111-115">An **Association** element in store schema definition language (SSDL) specifies table columns that participate in a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="74111-116">Due elementi End figlio obbligatorio specificano tabelle alle estremità dell'associazione e la molteplicità in ciascuna estremità.</span><span class="sxs-lookup"><span data-stu-id="74111-116">Two required child End elements specify tables at the ends of the association and the multiplicity at each end.</span></span> <span data-ttu-id="74111-117">Elemento ReferentialConstraint facoltativo specifica le entità finali principale e dipendente dell'associazione, nonché le colonne coinvolte.</span><span class="sxs-lookup"><span data-stu-id="74111-117">An optional ReferentialConstraint element specifies the principal and dependent ends of the association as well as the participating columns.</span></span> <span data-ttu-id="74111-118">Se nessun **ReferentialConstraint** è presente l'elemento, un elemento AssociationSetMapping deve essere usato per specificare i mapping delle colonne per l'associazione.</span><span class="sxs-lookup"><span data-stu-id="74111-118">If no **ReferentialConstraint** element is present, an AssociationSetMapping element must be used to specify the column mappings for the association.</span></span>

<span data-ttu-id="74111-119">Il **Association** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="74111-119">The **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="74111-120">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="74111-120">Documentation (zero or one)</span></span>
-   <span data-ttu-id="74111-121">End (esattamente due elementi)</span><span class="sxs-lookup"><span data-stu-id="74111-121">End (exactly two)</span></span>
-   <span data-ttu-id="74111-122">ReferentialConstraint (zero o più)</span><span class="sxs-lookup"><span data-stu-id="74111-122">ReferentialConstraint (zero or one)</span></span>
-   <span data-ttu-id="74111-123">Elementi Annotation (zero o più)</span><span class="sxs-lookup"><span data-stu-id="74111-123">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="74111-124">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="74111-124">Applicable Attributes</span></span>

<span data-ttu-id="74111-125">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **Association** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-125">The following table describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="74111-126">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="74111-126">Attribute Name</span></span> | <span data-ttu-id="74111-127">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="74111-127">Is Required</span></span> | <span data-ttu-id="74111-128">Valore</span><span class="sxs-lookup"><span data-stu-id="74111-128">Value</span></span>                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| <span data-ttu-id="74111-129">**Name**</span><span class="sxs-lookup"><span data-stu-id="74111-129">**Name**</span></span>       | <span data-ttu-id="74111-130">Yes</span><span class="sxs-lookup"><span data-stu-id="74111-130">Yes</span></span>         | <span data-ttu-id="74111-131">Il nome del vincolo di chiave esterna corrispondente nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="74111-131">The name of the corresponding foreign key constraint in the underlying database.</span></span> |

> [!NOTE]
> <span data-ttu-id="74111-132">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **Association** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-132">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="74111-133">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="74111-133">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="74111-134">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="74111-134">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="74111-135">Esempio</span><span class="sxs-lookup"><span data-stu-id="74111-135">Example</span></span>

<span data-ttu-id="74111-136">L'esempio seguente mostra un' **Association** elemento che utilizza un **ReferentialConstraint** elemento per specificare le colonne che fanno parte del **FK\_CustomerOrders**  vincolo di chiave esterna:</span><span class="sxs-lookup"><span data-stu-id="74111-136">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="associationset-element-ssdl"></a><span data-ttu-id="74111-137">Elemento AssociationSet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="74111-137">AssociationSet Element (SSDL)</span></span>

<span data-ttu-id="74111-138">Il **AssociationSet** elemento schema store schema definition language (SSDL) rappresenta un vincolo di chiave esterna tra due tabelle nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="74111-138">The **AssociationSet** element in store schema definition language (SSDL) represents a foreign key constraint between two tables in the underlying database.</span></span> <span data-ttu-id="74111-139">Le colonne della tabella che fanno parte del vincolo di chiave esterna vengono specificate in un elemento di associazione.</span><span class="sxs-lookup"><span data-stu-id="74111-139">The table columns that participate in the foreign key constraint are specified in an Association element.</span></span> <span data-ttu-id="74111-140">Il **associazione** elemento che corrisponde a un determinato **AssociationSet** viene specificato nell'elemento il **associazione** attributo del **AssociationSet**  elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-140">The **Association** element that corresponds to a given **AssociationSet** element is specified in the **Association** attribute of the **AssociationSet** element.</span></span>

<span data-ttu-id="74111-141">Set di associazioni SSDL viene eseguito il mapping al set di associazioni CSDL da un elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="74111-141">SSDL association sets are mapped to CSDL association sets by an AssociationSetMapping element.</span></span> <span data-ttu-id="74111-142">Tuttavia, se è impostata l'associazione CSDL per una determinata associazione CSDL è definito con un elemento ReferentialConstraint, non corrispondente **AssociationSetMapping** elemento è necessario.</span><span class="sxs-lookup"><span data-stu-id="74111-142">However, if the CSDL association for a given CSDL association set is defined by using a ReferentialConstraint element , no corresponding **AssociationSetMapping** element is necessary.</span></span> <span data-ttu-id="74111-143">In questo caso, se un' **AssociationSetMapping** l'elemento è presente, i mapping verranno sostituiti dalle **ReferentialConstraint** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-143">In this case, if an **AssociationSetMapping** element is present, the mappings it defines will be overridden by the **ReferentialConstraint** element.</span></span>

<span data-ttu-id="74111-144">Il **AssociationSet** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="74111-144">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="74111-145">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="74111-145">Documentation (zero or one)</span></span>
-   <span data-ttu-id="74111-146">End (zero o due)</span><span class="sxs-lookup"><span data-stu-id="74111-146">End (zero or two)</span></span>
-   <span data-ttu-id="74111-147">Elementi Annotation (zero o più)</span><span class="sxs-lookup"><span data-stu-id="74111-147">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="74111-148">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="74111-148">Applicable Attributes</span></span>

<span data-ttu-id="74111-149">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **AssociationSet** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-149">The following table describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="74111-150">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="74111-150">Attribute Name</span></span>  | <span data-ttu-id="74111-151">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="74111-151">Is Required</span></span> | <span data-ttu-id="74111-152">Valore</span><span class="sxs-lookup"><span data-stu-id="74111-152">Value</span></span>                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="74111-153">**Name**</span><span class="sxs-lookup"><span data-stu-id="74111-153">**Name**</span></span>        | <span data-ttu-id="74111-154">Yes</span><span class="sxs-lookup"><span data-stu-id="74111-154">Yes</span></span>         | <span data-ttu-id="74111-155">Nome del vincolo di chiave esterna rappresentato dal set di associazioni.</span><span class="sxs-lookup"><span data-stu-id="74111-155">The name of the foreign key constraint that the association set represents.</span></span>                          |
| <span data-ttu-id="74111-156">**Associazione**</span><span class="sxs-lookup"><span data-stu-id="74111-156">**Association**</span></span> | <span data-ttu-id="74111-157">Yes</span><span class="sxs-lookup"><span data-stu-id="74111-157">Yes</span></span>         | <span data-ttu-id="74111-158">Nome dell'associazione che definisce le colonne che fanno parte del vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="74111-158">The name of the association that defines the columns that participate in the foreign key constraint.</span></span> |

> [!NOTE]
> <span data-ttu-id="74111-159">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **AssociationSet** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-159">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="74111-160">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="74111-160">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="74111-161">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="74111-161">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="74111-162">Esempio</span><span class="sxs-lookup"><span data-stu-id="74111-162">Example</span></span>

<span data-ttu-id="74111-163">L'esempio seguente mostra un' **AssociationSet** elemento che rappresenta il `FK_CustomerOrders` vincolo di chiave esterna nel database sottostante:</span><span class="sxs-lookup"><span data-stu-id="74111-163">The following example shows an **AssociationSet** element that represents the `FK_CustomerOrders` foreign key constraint in the underlying database:</span></span>

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a><span data-ttu-id="74111-164">Elemento CollectionType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="74111-164">CollectionType Element (SSDL)</span></span>

<span data-ttu-id="74111-165">Il **CollectionType** elemento di schema store schema definition language (SSDL) specifica che il tipo restituito della funzione è una raccolta.</span><span class="sxs-lookup"><span data-stu-id="74111-165">The **CollectionType** element in store schema definition language (SSDL) specifies that a function’s return type is a collection.</span></span> <span data-ttu-id="74111-166">Il **CollectionType** elemento è figlio dell'elemento ReturnType.</span><span class="sxs-lookup"><span data-stu-id="74111-166">The **CollectionType** element is a child of the ReturnType element.</span></span> <span data-ttu-id="74111-167">Il tipo di raccolta viene specificato usando l'elemento figlio di tipo di riga:</span><span class="sxs-lookup"><span data-stu-id="74111-167">The type of collection is specified by using the RowType child element:</span></span>

> [!NOTE]
> <span data-ttu-id="74111-168">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **CollectionType** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-168">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="74111-169">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="74111-169">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="74111-170">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="74111-170">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="74111-171">Esempio</span><span class="sxs-lookup"><span data-stu-id="74111-171">Example</span></span>

<span data-ttu-id="74111-172">Nell'esempio seguente viene illustrata una funzione che usa un' **CollectionType** elemento per specificare che la funzione restituisce una raccolta di righe.</span><span class="sxs-lookup"><span data-stu-id="74111-172">The following example shows a function that uses a **CollectionType** element to specify that the function returns a collection of rows.</span></span>

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

## <a name="commandtext-element-ssdl"></a><span data-ttu-id="74111-173">Elemento CommandText (SSDL)</span><span class="sxs-lookup"><span data-stu-id="74111-173">CommandText Element (SSDL)</span></span>

<span data-ttu-id="74111-174">Il **CommandText** elemento schema store schema definition language (SSDL) è un figlio dell'elemento di funzione che consente di definire un'istruzione SQL che viene eseguita a livello di database.</span><span class="sxs-lookup"><span data-stu-id="74111-174">The **CommandText** element in store schema definition language (SSDL) is a child of the Function element that allows you to define a SQL statement that is executed at the database.</span></span> <span data-ttu-id="74111-175">Il **CommandText** elemento consente di aggiungere funzionalità simili a una stored procedure nel database, ma si definiscono le **CommandText** elemento nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="74111-175">The **CommandText** element allows you to add functionality that is similar to a stored procedure in the database, but you define the **CommandText** element in the storage model.</span></span>

<span data-ttu-id="74111-176">Il **CommandText** elemento non può avere elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="74111-176">The **CommandText** element cannot have child elements.</span></span> <span data-ttu-id="74111-177">Il corpo del **CommandText** elemento deve essere un'istruzione SQL valida per il database sottostante.</span><span class="sxs-lookup"><span data-stu-id="74111-177">The body of the **CommandText** element must be a valid SQL statement for the underlying database.</span></span>

<span data-ttu-id="74111-178">Attributi non sono applicabili per il **CommandText** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-178">No attributes are applicable to the **CommandText** element.</span></span>

### <a name="example"></a><span data-ttu-id="74111-179">Esempio</span><span class="sxs-lookup"><span data-stu-id="74111-179">Example</span></span>

<span data-ttu-id="74111-180">L'esempio seguente mostra una **funzione** elemento con un elemento figlio **CommandText** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-180">The following example shows a **Function** element with a child **CommandText** element.</span></span> <span data-ttu-id="74111-181">Esporre il **UpdateProductInOrder** fungere da un metodo in ObjectContext importandolo nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="74111-181">Expose the **UpdateProductInOrder** function as a method on the ObjectContext by importing it into the conceptual model.</span></span>  

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

## <a name="definingquery-element-ssdl"></a><span data-ttu-id="74111-182">Elemento DefiningQuery (SSDL)</span><span class="sxs-lookup"><span data-stu-id="74111-182">DefiningQuery Element (SSDL)</span></span>

<span data-ttu-id="74111-183">Il **DefiningQuery** elemento schema store schema definition language (SSDL) consente di eseguire un'istruzione SQL direttamente nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="74111-183">The **DefiningQuery** element in store schema definition language (SSDL) allows you to execute a SQL statement directly in the underlying database.</span></span> <span data-ttu-id="74111-184">Il **DefiningQuery** elemento viene comunemente usato, ad esempio una vista di database, ma la vista viene definita nel modello di archiviazione anziché nel database.</span><span class="sxs-lookup"><span data-stu-id="74111-184">The **DefiningQuery** element is commonly used like a database view, but the view is defined in the storage model instead of the database.</span></span> <span data-ttu-id="74111-185">La visualizzazione definita in un **DefiningQuery** elementi possono essere mappati a un tipo di entità nel modello concettuale tramite un elemento EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="74111-185">The view defined in a **DefiningQuery** element can be mapped to an entity type in the conceptual model through an EntitySetMapping element.</span></span> <span data-ttu-id="74111-186">Questi mapping sono di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="74111-186">These mappings are read-only.</span></span>  

<span data-ttu-id="74111-187">La sintassi SSDL seguente illustra la dichiarazione di un **EntitySet** aggiungendo il **DefiningQuery** elemento contenente una query usata per recuperare la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="74111-187">The following SSDL syntax shows the declaration of an **EntitySet** followed by the **DefiningQuery** element that contains a query used to retrieve the view.</span></span>

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

<span data-ttu-id="74111-188">È possibile usare le stored procedure in Entity Framework per abilitare scenari di lettura / scrittura sulle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="74111-188">You can use stored procedures in the Entity Framework to enable read-write scenarios over views.</span></span> <span data-ttu-id="74111-189">È possibile utilizzare una vista origine dati o una visualizzazione Entity SQL come tabella di base per il recupero dei dati e per l'elaborazione tramite le stored procedure delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="74111-189">You can use either a data source view or an Entity SQL view as the base table for retrieving data and for change processing by stored procedures.</span></span>

<span data-ttu-id="74111-190">È possibile usare la **DefiningQuery** elemento destinazione Microsoft SQL Server Compact 3.5.</span><span class="sxs-lookup"><span data-stu-id="74111-190">You can use the **DefiningQuery** element to target Microsoft SQL Server Compact 3.5.</span></span> <span data-ttu-id="74111-191">Anche se SQL Server Compact 3.5 non supporta stored procedure, è possibile implementare una funzionalità simile con il **DefiningQuery** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-191">Though SQL Server Compact 3.5 does not support stored procedures, you can implement similar functionality with the **DefiningQuery** element.</span></span> <span data-ttu-id="74111-192">Un'altra situazione in cui può essere utile è nella creazione di stored procedure per risolvere una mancata corrispondenza tra i tipi di dati utilizzati nel linguaggio di programmazione e quelli dell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="74111-192">Another place where it can be useful is in creating stored procedures to overcome a mismatch between the data types used in the programming language and those of the data source.</span></span> <span data-ttu-id="74111-193">È possibile scrivere un **DefiningQuery** che accetta un determinato set di parametri e quindi chiama una stored procedure con un diverso set di parametri, ad esempio, una stored procedure che elimina i dati.</span><span class="sxs-lookup"><span data-stu-id="74111-193">You could write a **DefiningQuery** that takes a certain set of parameters and then calls a stored procedure with a different set of parameters, for example, a stored procedure that deletes data.</span></span>

## <a name="dependent-element-ssdl"></a><span data-ttu-id="74111-194">Elemento Dependent (SSDL)</span><span class="sxs-lookup"><span data-stu-id="74111-194">Dependent Element (SSDL)</span></span>

<span data-ttu-id="74111-195">Il **dipendenti** in schema store schema definition language (SSDL) è un elemento figlio all'elemento ReferentialConstraint che definisce l'estremità dipendente del vincolo di chiave esterno (chiamato anche vincolo referenziale).</span><span class="sxs-lookup"><span data-stu-id="74111-195">The **Dependent** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the dependent end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="74111-196">Il **dipendenti** elemento specifica la colonna (o le colonne) in una tabella che fanno riferimento a una colonna chiave primaria (o colonne).</span><span class="sxs-lookup"><span data-stu-id="74111-196">The **Dependent** element specifies the column (or columns) in a table that reference a primary key column (or columns).</span></span> <span data-ttu-id="74111-197">**PropertyRef** elementi specificano quali colonne fanno riferimento.</span><span class="sxs-lookup"><span data-stu-id="74111-197">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="74111-198">Il principale elemento specifica le colonne chiave primaria cui fanno riferimento le colonne specificate nel **dipendenti** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-198">The Principal element specifies the primary key columns that are referenced by columns that are specified in the **Dependent** element.</span></span>

<span data-ttu-id="74111-199">Il **dipendenti** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="74111-199">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="74111-200">PropertyRef (una o più)</span><span class="sxs-lookup"><span data-stu-id="74111-200">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="74111-201">Elementi Annotation (zero o più)</span><span class="sxs-lookup"><span data-stu-id="74111-201">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="74111-202">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="74111-202">Applicable Attributes</span></span>

<span data-ttu-id="74111-203">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **dipendenti** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-203">The following table describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="74111-204">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="74111-204">Attribute Name</span></span> | <span data-ttu-id="74111-205">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="74111-205">Is Required</span></span> | <span data-ttu-id="74111-206">Valore</span><span class="sxs-lookup"><span data-stu-id="74111-206">Value</span></span>                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="74111-207">**Ruolo**</span><span class="sxs-lookup"><span data-stu-id="74111-207">**Role**</span></span>       | <span data-ttu-id="74111-208">Yes</span><span class="sxs-lookup"><span data-stu-id="74111-208">Yes</span></span>         | <span data-ttu-id="74111-209">Lo stesso valore di **ruolo** (se utilizzato) dell'attributo dell'elemento End corrispondente; in caso contrario, il nome della tabella contenente la colonna di riferimento.</span><span class="sxs-lookup"><span data-stu-id="74111-209">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referencing column.</span></span> |

> [!NOTE]
> <span data-ttu-id="74111-210">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **dipendenti** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-210">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="74111-211">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="74111-211">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="74111-212">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="74111-212">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="74111-213">Esempio</span><span class="sxs-lookup"><span data-stu-id="74111-213">Example</span></span>

<span data-ttu-id="74111-214">L'esempio seguente illustra un elemento di associazione che utilizza un **ReferentialConstraint** elemento per specificare le colonne che partecipano le **FK\_CustomerOrders** chiave esterna vincolo.</span><span class="sxs-lookup"><span data-stu-id="74111-214">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="74111-215">Il **dipendenti** elemento specifica il **CustomerId** colonna del **ordine** tabella come entità finale dipendente del vincolo.</span><span class="sxs-lookup"><span data-stu-id="74111-215">The **Dependent** element specifies the **CustomerId** column of the **Order** table as the dependent end of the constraint.</span></span>

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

## <a name="documentation-element-ssdl"></a><span data-ttu-id="74111-216">Elemento Documentation (SSDL)</span><span class="sxs-lookup"><span data-stu-id="74111-216">Documentation Element (SSDL)</span></span>

<span data-ttu-id="74111-217">Il **documentazione** elemento in schema store schema definition language (SSDL) può essere usato per fornire informazioni su un oggetto definito in un elemento padre.</span><span class="sxs-lookup"><span data-stu-id="74111-217">The **Documentation** element in store schema definition language (SSDL) can be used to provide information about an object that is defined in a parent element.</span></span>

<span data-ttu-id="74111-218">Il **documentazione** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="74111-218">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="74111-219">**Riepilogo**: una breve descrizione dell'elemento padre.</span><span class="sxs-lookup"><span data-stu-id="74111-219">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="74111-220">Zero o un elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-220">(zero or one element)</span></span>
-   <span data-ttu-id="74111-221">**LongDescription**: descrizione Luna dell'elemento padre.</span><span class="sxs-lookup"><span data-stu-id="74111-221">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="74111-222">Zero o un elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-222">(zero or one element)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="74111-223">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="74111-223">Applicable Attributes</span></span>

<span data-ttu-id="74111-224">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **documentazione** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-224">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="74111-225">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="74111-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="74111-226">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="74111-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="74111-227">Esempio</span><span class="sxs-lookup"><span data-stu-id="74111-227">Example</span></span>

<span data-ttu-id="74111-228">L'esempio seguente mostra le **documentazione** elemento come elemento figlio di un elemento EntityType.</span><span class="sxs-lookup"><span data-stu-id="74111-228">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span>

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

## <a name="end-element-ssdl"></a><span data-ttu-id="74111-229">Elemento End (SSDL)</span><span class="sxs-lookup"><span data-stu-id="74111-229">End Element (SSDL)</span></span>

<span data-ttu-id="74111-230">Il **End** elemento di schema store schema definition language (SSDL) specifica la tabella e il numero di righe in un'entità finale del vincolo di chiave esterno nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="74111-230">The **End** element in store schema definition language (SSDL) specifies the table and number of rows at one end of a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="74111-231">Il **End** elemento può essere un figlio dell'elemento di associazione o l'elemento AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="74111-231">The **End** element can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="74111-232">In entrambi i casi, gli elementi figlio possibili e gli attributi applicabili sono diversi.</span><span class="sxs-lookup"><span data-stu-id="74111-232">In each case, the possible child elements and applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="74111-233">Elemento End come figlio dell'elemento Association</span><span class="sxs-lookup"><span data-stu-id="74111-233">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="74111-234">Un' **End** elemento (come figlio del **Association** elemento) specifica la tabella e il numero di righe alla fine di un vincolo di chiave esterna con il **tipo** e**Molteplicità** rispettivamente gli attributi.</span><span class="sxs-lookup"><span data-stu-id="74111-234">An **End** element (as a child of the **Association** element) specifies the table and number of rows at the end of a foreign key constraint with the **Type** and **Multiplicity** attributes respectively.</span></span> <span data-ttu-id="74111-235">Le entità finali del vincolo di chiave esterna sono definite come parte dell'associazione SSDL. L'associazione SSDL deve disporre esattamente di due entità finali.</span><span class="sxs-lookup"><span data-stu-id="74111-235">Ends of a foreign key constraint are defined as part of an SSDL association; an SSDL association must have exactly two ends.</span></span>

<span data-ttu-id="74111-236">Un' **End** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="74111-236">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="74111-237">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="74111-237">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="74111-238">OnDelete (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="74111-238">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="74111-239">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="74111-239">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="74111-240">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="74111-240">Applicable Attributes</span></span>

<span data-ttu-id="74111-241">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **finali** elemento quando è figlio di un **associazione** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-241">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="74111-242">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="74111-242">Attribute Name</span></span>   | <span data-ttu-id="74111-243">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="74111-243">Is Required</span></span> | <span data-ttu-id="74111-244">Valore</span><span class="sxs-lookup"><span data-stu-id="74111-244">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="74111-245">**Type**</span><span class="sxs-lookup"><span data-stu-id="74111-245">**Type**</span></span>         | <span data-ttu-id="74111-246">Yes</span><span class="sxs-lookup"><span data-stu-id="74111-246">Yes</span></span>         | <span data-ttu-id="74111-247">Il nome completo del set di entità SSDL che si trova alla fine del vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="74111-247">The fully qualified name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                                                                                                                                                                                                                                                                          |
| <span data-ttu-id="74111-248">**Ruolo**</span><span class="sxs-lookup"><span data-stu-id="74111-248">**Role**</span></span>         | <span data-ttu-id="74111-249">No</span><span class="sxs-lookup"><span data-stu-id="74111-249">No</span></span>          | <span data-ttu-id="74111-250">Il valore della **ruolo** attributo nell'elemento principale o dipendente del corrispondente elemento ReferentialConstraint (se usato).</span><span class="sxs-lookup"><span data-stu-id="74111-250">The value of the **Role** attribute in either the Principal or Dependent element of the corresponding ReferentialConstraint element (if used).</span></span>                                                                                                                                                                                                                                             |
| <span data-ttu-id="74111-251">**Molteplicità**</span><span class="sxs-lookup"><span data-stu-id="74111-251">**Multiplicity**</span></span> | <span data-ttu-id="74111-252">Yes</span><span class="sxs-lookup"><span data-stu-id="74111-252">Yes</span></span>         | <span data-ttu-id="74111-253">**1**, **0..1**, o **\*** a seconda del numero di righe che possono essere alla fine del vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="74111-253">**1**, **0..1**, or **\*** depending on the number of rows that can be at the end of the foreign key constraint.</span></span> <br/> <span data-ttu-id="74111-254">**1** indica esattamente una riga presente alla fine vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="74111-254">**1** indicates that exactly one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="74111-255">**0..1** indica che una o nessuna riga presente alla fine vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="74111-255">**0..1** indicates that zero or one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="74111-256">**\*** indica che esistano zero, uno o più righe alla fine vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="74111-256">**\*** indicates that zero, one, or more rows exist at the foreign key constraint end.</span></span> |

> [!NOTE]
> <span data-ttu-id="74111-257">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **End** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-257">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="74111-258">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="74111-258">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="74111-259">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="74111-259">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="74111-260">Esempio</span><span class="sxs-lookup"><span data-stu-id="74111-260">Example</span></span>

<span data-ttu-id="74111-261">L'esempio seguente mostra un' **Association** elemento che definisce il **FK\_CustomerOrders** vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="74111-261">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="74111-262">Il **molteplicità** i valori specificati in ogni **End** elemento indicano che molte righe nel **ordini** può essere associata a una riga nella tabella il **clienti**  tabella, ma solo una riga nella **clienti** può essere associata a una riga nella tabella il **ordini** tabella.</span><span class="sxs-lookup"><span data-stu-id="74111-262">The **Multiplicity** values specified on each **End** element indicate that many rows in the **Orders** table can be associated with a row in the **Customers** table, but only one row in the **Customers** table can be associated with a row in the **Orders** table.</span></span> <span data-ttu-id="74111-263">Inoltre, il **OnDelete** elemento indica che tutte le righe nel **ordini** che fanno riferimento a una determinata riga nella tabella il **clienti** tabella verrà eliminata se la riga in il **clienti** tabella viene eliminata.</span><span class="sxs-lookup"><span data-stu-id="74111-263">Additionally, the **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="74111-264">Elemento End come figlio dell'elemento AssociationSet</span><span class="sxs-lookup"><span data-stu-id="74111-264">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="74111-265">Il **End** elemento (come figlio di **AssociationSet** elemento) specifica una tabella in un'entità finale del vincolo di chiave esterno nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="74111-265">The **End** element (as a child of the **AssociationSet** element) specifies a table at one end of a foreign key constraint in the underlying database.</span></span>

<span data-ttu-id="74111-266">Un' **End** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="74111-266">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="74111-267">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="74111-267">Documentation (zero or one)</span></span>
-   <span data-ttu-id="74111-268">Elementi Annotation (zero o più)</span><span class="sxs-lookup"><span data-stu-id="74111-268">Annotation elements (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="74111-269">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="74111-269">Applicable Attributes</span></span>

<span data-ttu-id="74111-270">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **finali** elemento quando è figlio di un **AssociationSet** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-270">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="74111-271">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="74111-271">Attribute Name</span></span> | <span data-ttu-id="74111-272">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="74111-272">Is Required</span></span> | <span data-ttu-id="74111-273">Valore</span><span class="sxs-lookup"><span data-stu-id="74111-273">Value</span></span>                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="74111-274">**Elemento EntitySet**</span><span class="sxs-lookup"><span data-stu-id="74111-274">**EntitySet**</span></span>  | <span data-ttu-id="74111-275">Yes</span><span class="sxs-lookup"><span data-stu-id="74111-275">Yes</span></span>         | <span data-ttu-id="74111-276">Il nome del set di entità SSDL che si trova alla fine del vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="74111-276">The name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                      |
| <span data-ttu-id="74111-277">**Ruolo**</span><span class="sxs-lookup"><span data-stu-id="74111-277">**Role**</span></span>       | <span data-ttu-id="74111-278">No</span><span class="sxs-lookup"><span data-stu-id="74111-278">No</span></span>          | <span data-ttu-id="74111-279">Il valore di uno dei **ruolo** gli attributi specificati in uno **End** elemento dell'elemento di associazione corrispondente.</span><span class="sxs-lookup"><span data-stu-id="74111-279">The value of one of the **Role** attributes specified on one **End** element of the corresponding Association element.</span></span> |

> [!NOTE]
> <span data-ttu-id="74111-280">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **End** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-280">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="74111-281">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="74111-281">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="74111-282">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="74111-282">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="74111-283">Esempio</span><span class="sxs-lookup"><span data-stu-id="74111-283">Example</span></span>

<span data-ttu-id="74111-284">L'esempio seguente mostra un' **EntityContainer** elemento con un **AssociationSet** elemento con due **End** elementi:</span><span class="sxs-lookup"><span data-stu-id="74111-284">The following example shows an **EntityContainer** element with an **AssociationSet** element with two **End** elements:</span></span>

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

## <a name="entitycontainer-element-ssdl"></a><span data-ttu-id="74111-285">Elemento EntityContainer (SSDL)</span><span class="sxs-lookup"><span data-stu-id="74111-285">EntityContainer Element (SSDL)</span></span>

<span data-ttu-id="74111-286">Un' **EntityContainer** elemento schema store schema definition language (SSDL) descrive la struttura dell'origine dati sottostante in un'applicazione Entity Framework: i set di entità SSDL (definiti negli elementi EntitySet) rappresentano le tabelle in un database, i tipi di entità SSDL (definiti negli elementi EntityType) rappresentano le righe in una tabella e set di associazioni (definiti negli elementi di AssociationSet) rappresentano vincoli di chiave esterna in un database.</span><span class="sxs-lookup"><span data-stu-id="74111-286">An **EntityContainer** element in store schema definition language (SSDL) describes the structure of the underlying data source in an Entity Framework application: SSDL entity sets (defined in EntitySet elements) represent tables in a database, SSDL entity types (defined in EntityType elements) represent rows in a table, and association sets (defined in AssociationSet elements) represent foreign key constraints in a database.</span></span> <span data-ttu-id="74111-287">Un contenitore di entità modello di archiviazione viene eseguito il mapping a un contenitore di entità del modello concettuale attraverso l'elemento EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="74111-287">A storage model entity container maps to a conceptual model entity container through the EntityContainerMapping element.</span></span>

<span data-ttu-id="74111-288">Un' **EntityContainer** elemento può avere zero o più elementi di documentazione.</span><span class="sxs-lookup"><span data-stu-id="74111-288">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="74111-289">Se un **documentazione** elemento è presente, deve precedere tutti gli elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="74111-289">If a **Documentation** element is present, it must precede all other child elements.</span></span>

<span data-ttu-id="74111-290">Un' **EntityContainer** elemento può includere zero o più degli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="74111-290">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="74111-291">EntitySet</span><span class="sxs-lookup"><span data-stu-id="74111-291">EntitySet</span></span>
-   <span data-ttu-id="74111-292">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="74111-292">AssociationSet</span></span>
-   <span data-ttu-id="74111-293">Elementi Annotation</span><span class="sxs-lookup"><span data-stu-id="74111-293">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="74111-294">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="74111-294">Applicable Attributes</span></span>

<span data-ttu-id="74111-295">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **EntityContainer** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-295">The table below describes the attributes that can be applied to the **EntityContainer** element.</span></span>

| <span data-ttu-id="74111-296">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="74111-296">Attribute Name</span></span> | <span data-ttu-id="74111-297">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="74111-297">Is Required</span></span> | <span data-ttu-id="74111-298">Valore</span><span class="sxs-lookup"><span data-stu-id="74111-298">Value</span></span>                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| <span data-ttu-id="74111-299">**Name**</span><span class="sxs-lookup"><span data-stu-id="74111-299">**Name**</span></span>       | <span data-ttu-id="74111-300">Yes</span><span class="sxs-lookup"><span data-stu-id="74111-300">Yes</span></span>         | <span data-ttu-id="74111-301">Nome del contenitore di entità.</span><span class="sxs-lookup"><span data-stu-id="74111-301">The name of the entity container.</span></span> <span data-ttu-id="74111-302">Il nome non può contenere caratteri punto (.).</span><span class="sxs-lookup"><span data-stu-id="74111-302">This name cannot contain periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="74111-303">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **EntityContainer** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-303">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="74111-304">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="74111-304">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="74111-305">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="74111-305">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="74111-306">Esempio</span><span class="sxs-lookup"><span data-stu-id="74111-306">Example</span></span>

<span data-ttu-id="74111-307">L'esempio seguente mostra un' **EntityContainer** elemento che definisce due set di entità e un set di associazioni.</span><span class="sxs-lookup"><span data-stu-id="74111-307">The following example shows an **EntityContainer** element that defines two entity sets and one association set.</span></span> <span data-ttu-id="74111-308">I nomi dei tipi di entità e associazione sono qualificati dal nome dello spazio dei nomi del modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="74111-308">Note that entity type and association type names are qualified by the conceptual model namespace name.</span></span>

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

## <a name="entityset-element-ssdl"></a><span data-ttu-id="74111-309">Elemento EntitySet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="74111-309">EntitySet Element (SSDL)</span></span>

<span data-ttu-id="74111-310">Un' **EntitySet** elemento schema store schema definition language (SSDL) rappresenta una tabella o vista nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="74111-310">An **EntitySet** element in store schema definition language (SSDL) represents a table or view in the underlying database.</span></span> <span data-ttu-id="74111-311">Un elemento EntityType nel linguaggio SSDL rappresenta una riga nella tabella o della vista.</span><span class="sxs-lookup"><span data-stu-id="74111-311">An EntityType element in SSDL represents a row in the table or view.</span></span> <span data-ttu-id="74111-312">Il **EntityType** attributo di un **EntitySet** elemento specifica il particolare tipo di entità SSDL che rappresenta le righe in un set di entità SSDL.</span><span class="sxs-lookup"><span data-stu-id="74111-312">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="74111-313">Il mapping tra un set di entità CSDL e un set di entità SSDL viene specificato in un elemento EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="74111-313">The mapping between a CSDL entity set and an SSDL entity set is specified in an EntitySetMapping element.</span></span>

<span data-ttu-id="74111-314">Il **EntitySet** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="74111-314">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="74111-315">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="74111-315">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="74111-316">DefiningQuery (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="74111-316">DefiningQuery (zero or one element)</span></span>
-   <span data-ttu-id="74111-317">Elementi Annotation</span><span class="sxs-lookup"><span data-stu-id="74111-317">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="74111-318">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="74111-318">Applicable Attributes</span></span>

<span data-ttu-id="74111-319">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **EntitySet** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-319">The following table describes the attributes that can be applied to the **EntitySet** element.</span></span>

> [!NOTE]
> <span data-ttu-id="74111-320">Alcuni attributi (non elencati qui) possono essere qualificati con il **archiviare** alias.</span><span class="sxs-lookup"><span data-stu-id="74111-320">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="74111-321">Questi attributi vengono utilizzati per la procedura guidata Aggiorna modello quando si aggiorna un modello.</span><span class="sxs-lookup"><span data-stu-id="74111-321">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="74111-322">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="74111-322">Attribute Name</span></span> | <span data-ttu-id="74111-323">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="74111-323">Is Required</span></span> | <span data-ttu-id="74111-324">Valore</span><span class="sxs-lookup"><span data-stu-id="74111-324">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="74111-325">**Name**</span><span class="sxs-lookup"><span data-stu-id="74111-325">**Name**</span></span>       | <span data-ttu-id="74111-326">Yes</span><span class="sxs-lookup"><span data-stu-id="74111-326">Yes</span></span>         | <span data-ttu-id="74111-327">Nome del set di entità.</span><span class="sxs-lookup"><span data-stu-id="74111-327">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="74111-328">**Elemento EntityType**</span><span class="sxs-lookup"><span data-stu-id="74111-328">**EntityType**</span></span> | <span data-ttu-id="74111-329">Yes</span><span class="sxs-lookup"><span data-stu-id="74111-329">Yes</span></span>         | <span data-ttu-id="74111-330">Nome completo del tipo di entità per il quale il set di entità contiene delle istanze.</span><span class="sxs-lookup"><span data-stu-id="74111-330">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |
| <span data-ttu-id="74111-331">**Schema**</span><span class="sxs-lookup"><span data-stu-id="74111-331">**Schema**</span></span>     | <span data-ttu-id="74111-332">No</span><span class="sxs-lookup"><span data-stu-id="74111-332">No</span></span>          | <span data-ttu-id="74111-333">Schema del database.</span><span class="sxs-lookup"><span data-stu-id="74111-333">The database schema.</span></span>                                                                     |
| <span data-ttu-id="74111-334">**Tabella**</span><span class="sxs-lookup"><span data-stu-id="74111-334">**Table**</span></span>      | <span data-ttu-id="74111-335">No</span><span class="sxs-lookup"><span data-stu-id="74111-335">No</span></span>          | <span data-ttu-id="74111-336">Tabella del database.</span><span class="sxs-lookup"><span data-stu-id="74111-336">The database table.</span></span>                                                                      |

> [!NOTE]
> <span data-ttu-id="74111-337">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **EntitySet** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-337">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="74111-338">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="74111-338">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="74111-339">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="74111-339">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="74111-340">Esempio</span><span class="sxs-lookup"><span data-stu-id="74111-340">Example</span></span>

<span data-ttu-id="74111-341">L'esempio seguente mostra un' **EntityContainer** elemento che dispone di due **EntitySet** elementi e uno **AssociationSet** elemento:</span><span class="sxs-lookup"><span data-stu-id="74111-341">The following example shows an **EntityContainer** element that has two **EntitySet** elements and one **AssociationSet** element:</span></span>

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

## <a name="entitytype-element-ssdl"></a><span data-ttu-id="74111-342">Elemento EntityType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="74111-342">EntityType Element (SSDL)</span></span>

<span data-ttu-id="74111-343">Un' **EntityType** elemento schema store schema definition language (SSDL) rappresenta una riga in una tabella o vista del database sottostante.</span><span class="sxs-lookup"><span data-stu-id="74111-343">An **EntityType** element in store schema definition language (SSDL) represents a row in a table or view of the underlying database.</span></span> <span data-ttu-id="74111-344">Un elemento EntitySet in SSDL rappresenta la tabella o vista in cui si trova la riga.</span><span class="sxs-lookup"><span data-stu-id="74111-344">An EntitySet element in SSDL represents the table or view in which rows occur.</span></span> <span data-ttu-id="74111-345">Il **EntityType** attributo di un **EntitySet** elemento specifica il particolare tipo di entità SSDL che rappresenta le righe in un set di entità SSDL.</span><span class="sxs-lookup"><span data-stu-id="74111-345">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="74111-346">Il mapping tra un tipo di entità SSDL e un tipo di entità CSDL viene specificato in un elemento EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="74111-346">The mapping between an SSDL entity type and a CSDL entity type is specified in an EntityTypeMapping element.</span></span>

<span data-ttu-id="74111-347">Il **EntityType** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="74111-347">The **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="74111-348">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="74111-348">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="74111-349">Chiave (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="74111-349">Key (zero or one element)</span></span>
-   <span data-ttu-id="74111-350">Elementi Annotation</span><span class="sxs-lookup"><span data-stu-id="74111-350">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="74111-351">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="74111-351">Applicable Attributes</span></span>

<span data-ttu-id="74111-352">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **EntityType** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-352">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="74111-353">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="74111-353">Attribute Name</span></span> | <span data-ttu-id="74111-354">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="74111-354">Is Required</span></span> | <span data-ttu-id="74111-355">Valore</span><span class="sxs-lookup"><span data-stu-id="74111-355">Value</span></span>                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="74111-356">**Name**</span><span class="sxs-lookup"><span data-stu-id="74111-356">**Name**</span></span>       | <span data-ttu-id="74111-357">Yes</span><span class="sxs-lookup"><span data-stu-id="74111-357">Yes</span></span>         | <span data-ttu-id="74111-358">Nome del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="74111-358">The name of the entity type.</span></span> <span data-ttu-id="74111-359">Questo valore corrisponde generalmente al nome della tabella in cui il tipo di entità rappresenta una riga.</span><span class="sxs-lookup"><span data-stu-id="74111-359">This value is usually the same as the name of the table in which the entity type represents a row.</span></span> <span data-ttu-id="74111-360">Questo valore non può contenere punti (.).</span><span class="sxs-lookup"><span data-stu-id="74111-360">This value can contain no periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="74111-361">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **EntityType** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-361">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="74111-362">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="74111-362">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="74111-363">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="74111-363">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="74111-364">Esempio</span><span class="sxs-lookup"><span data-stu-id="74111-364">Example</span></span>

<span data-ttu-id="74111-365">L'esempio seguente mostra un' **EntityType** elemento con due proprietà:</span><span class="sxs-lookup"><span data-stu-id="74111-365">The following example shows an **EntityType** element with two properties:</span></span>

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

## <a name="function-element-ssdl"></a><span data-ttu-id="74111-366">Elemento Function (SSDL)</span><span class="sxs-lookup"><span data-stu-id="74111-366">Function Element (SSDL)</span></span>

<span data-ttu-id="74111-367">Il **funzione** elemento di schema store schema definition language (SSDL) specifica una stored procedure esistente nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="74111-367">The **Function** element in store schema definition language (SSDL) specifies a stored procedure that exists in the underlying database.</span></span>

<span data-ttu-id="74111-368">Il **funzione** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="74111-368">The **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="74111-369">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="74111-369">Documentation (zero or one)</span></span>
-   <span data-ttu-id="74111-370">Parametro (zero o più)</span><span class="sxs-lookup"><span data-stu-id="74111-370">Parameter (zero or more)</span></span>
-   <span data-ttu-id="74111-371">CommandText (zero o più)</span><span class="sxs-lookup"><span data-stu-id="74111-371">CommandText (zero or one)</span></span>
-   <span data-ttu-id="74111-372">ReturnType (zero o più)</span><span class="sxs-lookup"><span data-stu-id="74111-372">ReturnType (zero or more)</span></span>
-   <span data-ttu-id="74111-373">Elementi Annotation (zero o più)</span><span class="sxs-lookup"><span data-stu-id="74111-373">Annotation elements (zero or more)</span></span>

<span data-ttu-id="74111-374">Restituisce un tipo per una funzione deve essere specificato mediante il **ReturnType** elemento o il **ReturnType** attributo (vedere sotto), ma non entrambi.</span><span class="sxs-lookup"><span data-stu-id="74111-374">A return type for a function must be specified with either the **ReturnType** element or the **ReturnType** attribute (see below), but not both.</span></span>

<span data-ttu-id="74111-375">È possibile importare stored procedure specificate nel modello di archiviazione nel modello concettuale di un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="74111-375">Stored procedures that are specified in the storage model can be imported into the conceptual model of an application.</span></span> <span data-ttu-id="74111-376">Per altre informazioni, vedere [esecuzione di query con Stored procedure](~/ef6/modeling/designer/stored-procedures/query.md).</span><span class="sxs-lookup"><span data-stu-id="74111-376">For more information, see [Querying with Stored Procedures](~/ef6/modeling/designer/stored-procedures/query.md).</span></span> <span data-ttu-id="74111-377">Il **funzione** elemento può essere usato anche per definire funzioni personalizzate nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="74111-377">The **Function** element can also be used to define custom functions in the storage model.</span></span>  

### <a name="applicable-attributes"></a><span data-ttu-id="74111-378">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="74111-378">Applicable Attributes</span></span>

<span data-ttu-id="74111-379">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **funzione** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-379">The following table describes the attributes that can be applied to the **Function** element.</span></span>

> [!NOTE]
> <span data-ttu-id="74111-380">Alcuni attributi (non elencati qui) possono essere qualificati con il **archiviare** alias.</span><span class="sxs-lookup"><span data-stu-id="74111-380">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="74111-381">Questi attributi vengono utilizzati per la procedura guidata Aggiorna modello quando si aggiorna un modello.</span><span class="sxs-lookup"><span data-stu-id="74111-381">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="74111-382">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="74111-382">Attribute Name</span></span>             | <span data-ttu-id="74111-383">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="74111-383">Is Required</span></span> | <span data-ttu-id="74111-384">Valore</span><span class="sxs-lookup"><span data-stu-id="74111-384">Value</span></span>                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="74111-385">**Name**</span><span class="sxs-lookup"><span data-stu-id="74111-385">**Name**</span></span>                   | <span data-ttu-id="74111-386">Yes</span><span class="sxs-lookup"><span data-stu-id="74111-386">Yes</span></span>         | <span data-ttu-id="74111-387">Nome della stored procedure.</span><span class="sxs-lookup"><span data-stu-id="74111-387">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="74111-388">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="74111-388">**ReturnType**</span></span>             | <span data-ttu-id="74111-389">No</span><span class="sxs-lookup"><span data-stu-id="74111-389">No</span></span>          | <span data-ttu-id="74111-390">Tipo restituito della stored procedure.</span><span class="sxs-lookup"><span data-stu-id="74111-390">The return type of the stored procedure.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="74111-391">**Aggregate**</span><span class="sxs-lookup"><span data-stu-id="74111-391">**Aggregate**</span></span>              | <span data-ttu-id="74111-392">No</span><span class="sxs-lookup"><span data-stu-id="74111-392">No</span></span>          | <span data-ttu-id="74111-393">**True** se la stored procedure restituisce un valore di aggregazione; in caso contrario **False**.</span><span class="sxs-lookup"><span data-stu-id="74111-393">**True** if the stored procedure returns an aggregate value; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="74111-394">**BuiltIn**</span><span class="sxs-lookup"><span data-stu-id="74111-394">**BuiltIn**</span></span>                | <span data-ttu-id="74111-395">No</span><span class="sxs-lookup"><span data-stu-id="74111-395">No</span></span>          | <span data-ttu-id="74111-396">**True** se la funzione è un oggetto incorporato<sup>1</sup> funzione; in caso contrario **False**.</span><span class="sxs-lookup"><span data-stu-id="74111-396">**True** if the function is a built-in<sup>1</sup> function; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="74111-397">**StoreFunctionName**</span><span class="sxs-lookup"><span data-stu-id="74111-397">**StoreFunctionName**</span></span>      | <span data-ttu-id="74111-398">No</span><span class="sxs-lookup"><span data-stu-id="74111-398">No</span></span>          | <span data-ttu-id="74111-399">Nome della stored procedure.</span><span class="sxs-lookup"><span data-stu-id="74111-399">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="74111-400">**NiladicFunction**</span><span class="sxs-lookup"><span data-stu-id="74111-400">**NiladicFunction**</span></span>        | <span data-ttu-id="74111-401">No</span><span class="sxs-lookup"><span data-stu-id="74111-401">No</span></span>          | <span data-ttu-id="74111-402">**True** se la funzione è senza parametri<sup>2</sup> funzione; **False** in caso contrario.</span><span class="sxs-lookup"><span data-stu-id="74111-402">**True** if the function is a niladic<sup>2</sup> function; **False** otherwise.</span></span>                                                                                                                                   |
| <span data-ttu-id="74111-403">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="74111-403">**IsComposable**</span></span>           | <span data-ttu-id="74111-404">No</span><span class="sxs-lookup"><span data-stu-id="74111-404">No</span></span>          | <span data-ttu-id="74111-405">**True** se la funzione è componibile<sup>3</sup> funzione; **False** in caso contrario.</span><span class="sxs-lookup"><span data-stu-id="74111-405">**True** if the function is a composable<sup>3</sup> function; **False** otherwise.</span></span>                                                                                                                                |
| <span data-ttu-id="74111-406">**ParameterTypeSemantics**</span><span class="sxs-lookup"><span data-stu-id="74111-406">**ParameterTypeSemantics**</span></span> | <span data-ttu-id="74111-407">No</span><span class="sxs-lookup"><span data-stu-id="74111-407">No</span></span>          | <span data-ttu-id="74111-408">Enumerazione che definisce la semantica dei tipi utilizzata per risolvere gli overload della funzione.</span><span class="sxs-lookup"><span data-stu-id="74111-408">The enumeration that defines the type semantics used to resolve function overloads.</span></span> <span data-ttu-id="74111-409">L'enumerazione è definita nel file manifesto del provider per ogni definizione di funzione.</span><span class="sxs-lookup"><span data-stu-id="74111-409">The enumeration is defined in the provider manifest per function definition.</span></span> <span data-ttu-id="74111-410">Il valore predefinito è **AllowImplicitConversion**.</span><span class="sxs-lookup"><span data-stu-id="74111-410">The default value is **AllowImplicitConversion**.</span></span> |
| <span data-ttu-id="74111-411">**Schema**</span><span class="sxs-lookup"><span data-stu-id="74111-411">**Schema**</span></span>                 | <span data-ttu-id="74111-412">No</span><span class="sxs-lookup"><span data-stu-id="74111-412">No</span></span>          | <span data-ttu-id="74111-413">Nome dello schema in cui viene definita la stored procedure.</span><span class="sxs-lookup"><span data-stu-id="74111-413">The name of the schema in which the stored procedure is defined.</span></span>                                                                                                                                                   |

<span data-ttu-id="74111-414"><sup>1</sup> una funzione predefinita è una funzione definita nel database.</span><span class="sxs-lookup"><span data-stu-id="74111-414"><sup>1</sup> A built-in function is a function that is defined in the database.</span></span> <span data-ttu-id="74111-415">Per informazioni sulle funzioni definite nel modello di archiviazione, vedere l'elemento CommandText (SSDL).</span><span class="sxs-lookup"><span data-stu-id="74111-415">For information about functions that are defined in the storage model, see CommandText Element (SSDL).</span></span>

<span data-ttu-id="74111-416"><sup>2</sup> una funzione senza parametri è una funzione che non accetta parametri e, quando viene chiamato, non richiede le parentesi.</span><span class="sxs-lookup"><span data-stu-id="74111-416"><sup>2</sup> A niladic function is a function that accepts no parameters and, when called, does not require parentheses.</span></span>

<span data-ttu-id="74111-417"><sup>3</sup> due funzioni sono componibili se l'output di una funzione può essere l'input per l'altra funzione.</span><span class="sxs-lookup"><span data-stu-id="74111-417"><sup>3</sup> Two functions are composable if the output of one function can be the input for the other function.</span></span>

> [!NOTE]
> <span data-ttu-id="74111-418">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **funzione** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-418">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="74111-419">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="74111-419">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="74111-420">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="74111-420">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="74111-421">Esempio</span><span class="sxs-lookup"><span data-stu-id="74111-421">Example</span></span>

<span data-ttu-id="74111-422">Nell'esempio seguente un **funzione** elemento che corrisponde al **UpdateOrderQuantity** stored procedure.</span><span class="sxs-lookup"><span data-stu-id="74111-422">The following example shows a **Function** element that corresponds to the **UpdateOrderQuantity** stored procedure.</span></span> <span data-ttu-id="74111-423">La stored procedure accetta due parametri e non restituisce alcun valore.</span><span class="sxs-lookup"><span data-stu-id="74111-423">The stored procedure accepts two parameters and does not return a value.</span></span>

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

## <a name="key-element-ssdl"></a><span data-ttu-id="74111-424">Elemento Key (SSDL)</span><span class="sxs-lookup"><span data-stu-id="74111-424">Key Element (SSDL)</span></span>

<span data-ttu-id="74111-425">Il **chiave** elemento schema store schema definition language (SSDL) rappresenta la chiave primaria di una tabella nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="74111-425">The **Key** element in store schema definition language (SSDL) represents the primary key of a table in the underlying database.</span></span> <span data-ttu-id="74111-426">**Chiave** è un elemento figlio di un elemento EntityType che rappresenta una riga in una tabella.</span><span class="sxs-lookup"><span data-stu-id="74111-426">**Key** is a child element of an EntityType element, which represents a row in a table.</span></span> <span data-ttu-id="74111-427">La chiave primaria è definita nel **Key** elemento facendo riferimento a uno o più elementi di proprietà definite nel **EntityType** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-427">The primary key is defined in the **Key** element by referencing one or more Property elements that are defined on the **EntityType** element.</span></span>

<span data-ttu-id="74111-428">Il **chiave** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="74111-428">The **Key** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="74111-429">PropertyRef (una o più)</span><span class="sxs-lookup"><span data-stu-id="74111-429">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="74111-430">Elementi Annotation</span><span class="sxs-lookup"><span data-stu-id="74111-430">Annotation elements</span></span>

<span data-ttu-id="74111-431">Attributi non sono applicabili per il **chiave** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-431">No attributes are applicable to the **Key** element.</span></span>

### <a name="example"></a><span data-ttu-id="74111-432">Esempio</span><span class="sxs-lookup"><span data-stu-id="74111-432">Example</span></span>

<span data-ttu-id="74111-433">L'esempio seguente mostra un' **EntityType** elemento con una chiave che fa riferimento a una proprietà:</span><span class="sxs-lookup"><span data-stu-id="74111-433">The following example shows an **EntityType** element with a key that references one property:</span></span>

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

## <a name="ondelete-element-ssdl"></a><span data-ttu-id="74111-434">Elemento OnDelete (SSDL)</span><span class="sxs-lookup"><span data-stu-id="74111-434">OnDelete Element (SSDL)</span></span>

<span data-ttu-id="74111-435">Il **OnDelete** elemento schema store schema definition language (SSDL) riflette il comportamento del database quando viene eliminata una riga che fa parte di un vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="74111-435">The **OnDelete** element in store schema definition language (SSDL) reflects the database behavior when a row that participates in a foreign key constraint is deleted.</span></span> <span data-ttu-id="74111-436">Se l'azione viene impostata su **Cascade**, quindi verranno eliminate anche le righe che fanno riferimento a una riga in fase di eliminazione.</span><span class="sxs-lookup"><span data-stu-id="74111-436">If the action is set to **Cascade**, then rows that reference a row that is being deleted will also be deleted.</span></span> <span data-ttu-id="74111-437">Se l'azione viene impostata su **None**, quindi non vengono eliminate anche le righe che fanno riferimento a una riga in fase di eliminazione.</span><span class="sxs-lookup"><span data-stu-id="74111-437">If the action is set to **None**, then rows that reference a row that is being deleted are not also deleted.</span></span> <span data-ttu-id="74111-438">Un' **OnDelete** è un elemento figlio dell'elemento finale.</span><span class="sxs-lookup"><span data-stu-id="74111-438">An **OnDelete** element is a child element of an End element.</span></span>

<span data-ttu-id="74111-439">Un' **OnDelete** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="74111-439">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="74111-440">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="74111-440">Documentation (zero or one)</span></span>
-   <span data-ttu-id="74111-441">Elementi Annotation (zero o più)</span><span class="sxs-lookup"><span data-stu-id="74111-441">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="74111-442">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="74111-442">Applicable Attributes</span></span>

<span data-ttu-id="74111-443">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **OnDelete** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-443">The following table describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="74111-444">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="74111-444">Attribute Name</span></span> | <span data-ttu-id="74111-445">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="74111-445">Is Required</span></span> | <span data-ttu-id="74111-446">Valore</span><span class="sxs-lookup"><span data-stu-id="74111-446">Value</span></span>                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="74111-447">**Azione**</span><span class="sxs-lookup"><span data-stu-id="74111-447">**Action**</span></span>     | <span data-ttu-id="74111-448">Yes</span><span class="sxs-lookup"><span data-stu-id="74111-448">Yes</span></span>         | <span data-ttu-id="74111-449">**CASCADE** oppure **None**.</span><span class="sxs-lookup"><span data-stu-id="74111-449">**Cascade** or **None**.</span></span> <span data-ttu-id="74111-450">(Il valore **Restricted** è valido ma presenta lo stesso comportamento **None**.)</span><span class="sxs-lookup"><span data-stu-id="74111-450">(The value **Restricted** is valid but has the same behavior as **None**.)</span></span> |

> [!NOTE]
> <span data-ttu-id="74111-451">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **OnDelete** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-451">Any number of annotation attributes (custom XML attributes) may be applied to the **OnDelete** element.</span></span> <span data-ttu-id="74111-452">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="74111-452">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="74111-453">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="74111-453">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="74111-454">Esempio</span><span class="sxs-lookup"><span data-stu-id="74111-454">Example</span></span>

<span data-ttu-id="74111-455">L'esempio seguente mostra un' **Association** elemento che definisce il **FK\_CustomerOrders** vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="74111-455">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="74111-456">Il **OnDelete** elemento indica che tutte le righe nel **ordini** che fanno riferimento a una determinata riga nella tabella di **clienti** tabella verrà eliminata se la riga nella finestra di **Clienti** tabella viene eliminata.</span><span class="sxs-lookup"><span data-stu-id="74111-456">The **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

## <a name="parameter-element-ssdl"></a><span data-ttu-id="74111-457">Elemento Parameter (SSDL)</span><span class="sxs-lookup"><span data-stu-id="74111-457">Parameter Element (SSDL)</span></span>

<span data-ttu-id="74111-458">Il **parametro** elemento schema store schema definition language (SSDL) è un figlio dell'elemento di funzione che specifica i parametri per una stored procedure nel database.</span><span class="sxs-lookup"><span data-stu-id="74111-458">The **Parameter** element in store schema definition language (SSDL) is a child of the Function element that specifies parameters for a stored procedure in the database.</span></span>

<span data-ttu-id="74111-459">Il **parametro** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="74111-459">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="74111-460">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="74111-460">Documentation (zero or one)</span></span>
-   <span data-ttu-id="74111-461">Elementi Annotation (zero o più)</span><span class="sxs-lookup"><span data-stu-id="74111-461">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="74111-462">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="74111-462">Applicable Attributes</span></span>

<span data-ttu-id="74111-463">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **parametro** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-463">The table below describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="74111-464">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="74111-464">Attribute Name</span></span> | <span data-ttu-id="74111-465">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="74111-465">Is Required</span></span> | <span data-ttu-id="74111-466">Valore</span><span class="sxs-lookup"><span data-stu-id="74111-466">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="74111-467">**Name**</span><span class="sxs-lookup"><span data-stu-id="74111-467">**Name**</span></span>       | <span data-ttu-id="74111-468">Yes</span><span class="sxs-lookup"><span data-stu-id="74111-468">Yes</span></span>         | <span data-ttu-id="74111-469">Nome del parametro.</span><span class="sxs-lookup"><span data-stu-id="74111-469">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="74111-470">**Type**</span><span class="sxs-lookup"><span data-stu-id="74111-470">**Type**</span></span>       | <span data-ttu-id="74111-471">Yes</span><span class="sxs-lookup"><span data-stu-id="74111-471">Yes</span></span>         | <span data-ttu-id="74111-472">Tipo del parametro.</span><span class="sxs-lookup"><span data-stu-id="74111-472">The parameter type.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="74111-473">**Modalità**</span><span class="sxs-lookup"><span data-stu-id="74111-473">**Mode**</span></span>       | <span data-ttu-id="74111-474">No</span><span class="sxs-lookup"><span data-stu-id="74111-474">No</span></span>          | <span data-ttu-id="74111-475">**Nelle**, **Out**, o **InOut** a seconda che il parametro sia un input, output o parametro di input/output.</span><span class="sxs-lookup"><span data-stu-id="74111-475">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="74111-476">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="74111-476">**MaxLength**</span></span>  | <span data-ttu-id="74111-477">No</span><span class="sxs-lookup"><span data-stu-id="74111-477">No</span></span>          | <span data-ttu-id="74111-478">Lunghezza massima del parametro.</span><span class="sxs-lookup"><span data-stu-id="74111-478">The maximum length of the parameter.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="74111-479">**Precisione**</span><span class="sxs-lookup"><span data-stu-id="74111-479">**Precision**</span></span>  | <span data-ttu-id="74111-480">No</span><span class="sxs-lookup"><span data-stu-id="74111-480">No</span></span>          | <span data-ttu-id="74111-481">Precisione del parametro.</span><span class="sxs-lookup"><span data-stu-id="74111-481">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="74111-482">**Scala**</span><span class="sxs-lookup"><span data-stu-id="74111-482">**Scale**</span></span>      | <span data-ttu-id="74111-483">No</span><span class="sxs-lookup"><span data-stu-id="74111-483">No</span></span>          | <span data-ttu-id="74111-484">Scala del parametro.</span><span class="sxs-lookup"><span data-stu-id="74111-484">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="74111-485">**SRID**</span><span class="sxs-lookup"><span data-stu-id="74111-485">**SRID**</span></span>       | <span data-ttu-id="74111-486">No</span><span class="sxs-lookup"><span data-stu-id="74111-486">No</span></span>          | <span data-ttu-id="74111-487">Identificatore di riferimento spaziale del sistema.</span><span class="sxs-lookup"><span data-stu-id="74111-487">Spatial System Reference Identifier.</span></span> <span data-ttu-id="74111-488">Valido solo per i parametri di tipi spaziali.</span><span class="sxs-lookup"><span data-stu-id="74111-488">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="74111-489">Per altre informazioni, vedere [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="74111-489">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

> [!NOTE]
> <span data-ttu-id="74111-490">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **parametro** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-490">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="74111-491">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="74111-491">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="74111-492">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="74111-492">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="74111-493">Esempio</span><span class="sxs-lookup"><span data-stu-id="74111-493">Example</span></span>

<span data-ttu-id="74111-494">L'esempio seguente mostra una **funzione** elemento che dispone di due **parametro** gli elementi che specificano i parametri di input:</span><span class="sxs-lookup"><span data-stu-id="74111-494">The following example shows a **Function** element that has two **Parameter** elements that specify input parameters:</span></span>

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

## <a name="principal-element-ssdl"></a><span data-ttu-id="74111-495">Elemento Principal (SSDL)</span><span class="sxs-lookup"><span data-stu-id="74111-495">Principal Element (SSDL)</span></span>

<span data-ttu-id="74111-496">Il **Principal** in schema store schema definition language (SSDL) è un elemento figlio all'elemento ReferentialConstraint che definisce l'entità finale del vincolo di chiave esterno (chiamato anche vincolo referenziale).</span><span class="sxs-lookup"><span data-stu-id="74111-496">The **Principal** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the principal end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="74111-497">Il **Principal** elemento specifica la colonna chiave primaria (o le colonne) in una tabella cui viene fatto riferimento da un'altra colonna (o colonne).</span><span class="sxs-lookup"><span data-stu-id="74111-497">The **Principal** element specifies the primary key column (or columns) in a table that is referenced by another column (or columns).</span></span> <span data-ttu-id="74111-498">**PropertyRef** elementi specificano quali colonne fanno riferimento.</span><span class="sxs-lookup"><span data-stu-id="74111-498">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="74111-499">Elemento Dependent specifica le colonne che fanno riferimento le colonne chiave primaria specificati nel **Principal** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-499">The Dependent element specifies columns that reference the primary key columns that are specified in the **Principal** element.</span></span>

<span data-ttu-id="74111-500">Il **Principal** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="74111-500">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="74111-501">PropertyRef (una o più)</span><span class="sxs-lookup"><span data-stu-id="74111-501">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="74111-502">Elementi Annotation (zero o più)</span><span class="sxs-lookup"><span data-stu-id="74111-502">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="74111-503">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="74111-503">Applicable Attributes</span></span>

<span data-ttu-id="74111-504">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **Principal** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-504">The following table describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="74111-505">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="74111-505">Attribute Name</span></span> | <span data-ttu-id="74111-506">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="74111-506">Is Required</span></span> | <span data-ttu-id="74111-507">Valore</span><span class="sxs-lookup"><span data-stu-id="74111-507">Value</span></span>                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="74111-508">**Ruolo**</span><span class="sxs-lookup"><span data-stu-id="74111-508">**Role**</span></span>       | <span data-ttu-id="74111-509">Yes</span><span class="sxs-lookup"><span data-stu-id="74111-509">Yes</span></span>         | <span data-ttu-id="74111-510">Lo stesso valore di **ruolo** (se utilizzato) dell'attributo dell'elemento End corrispondente; in caso contrario, il nome della tabella contenente la colonna di riferimento.</span><span class="sxs-lookup"><span data-stu-id="74111-510">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referenced column.</span></span> |

> [!NOTE]
> <span data-ttu-id="74111-511">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **Principal** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-511">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="74111-512">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="74111-512">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="74111-513">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="74111-513">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="74111-514">Esempio</span><span class="sxs-lookup"><span data-stu-id="74111-514">Example</span></span>

<span data-ttu-id="74111-515">L'esempio seguente illustra un elemento di associazione che utilizza un **ReferentialConstraint** elemento per specificare le colonne che partecipano le **FK\_CustomerOrders** chiave esterna vincolo.</span><span class="sxs-lookup"><span data-stu-id="74111-515">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="74111-516">Il **dell'entità** elemento specifica il **CustomerId** colonna del **cliente** tabella come entità finale principale del vincolo.</span><span class="sxs-lookup"><span data-stu-id="74111-516">The **Principal** element specifies the **CustomerId** column of the **Customer** table as the principal end of the constraint.</span></span>

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

## <a name="property-element-ssdl"></a><span data-ttu-id="74111-517">Elemento Property (SSDL)</span><span class="sxs-lookup"><span data-stu-id="74111-517">Property Element (SSDL)</span></span>

<span data-ttu-id="74111-518">Il **proprietà** elemento schema store schema definition language (SSDL) rappresenta una colonna in una tabella nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="74111-518">The **Property** element in store schema definition language (SSDL) represents a column in a table in the underlying database.</span></span> <span data-ttu-id="74111-519">**Proprietà** elementi sono elementi figlio degli elementi EntityType, che rappresentano le righe in una tabella.</span><span class="sxs-lookup"><span data-stu-id="74111-519">**Property** elements are children of EntityType elements, which represent rows in a table.</span></span> <span data-ttu-id="74111-520">Ciascuna **proprietà** elemento definito in un **EntityType** elemento rappresenta una colonna.</span><span class="sxs-lookup"><span data-stu-id="74111-520">Each **Property** element defined on an **EntityType** element represents a column.</span></span>

<span data-ttu-id="74111-521">Oggetto **proprietà** elemento non può avere elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="74111-521">A **Property** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="74111-522">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="74111-522">Applicable Attributes</span></span>

<span data-ttu-id="74111-523">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **proprietà** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-523">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="74111-524">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="74111-524">Attribute Name</span></span>            | <span data-ttu-id="74111-525">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="74111-525">Is Required</span></span> | <span data-ttu-id="74111-526">Valore</span><span class="sxs-lookup"><span data-stu-id="74111-526">Value</span></span>                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="74111-527">**Name**</span><span class="sxs-lookup"><span data-stu-id="74111-527">**Name**</span></span>                  | <span data-ttu-id="74111-528">Yes</span><span class="sxs-lookup"><span data-stu-id="74111-528">Yes</span></span>         | <span data-ttu-id="74111-529">Nome della colonna corrispondente.</span><span class="sxs-lookup"><span data-stu-id="74111-529">The name of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="74111-530">**Type**</span><span class="sxs-lookup"><span data-stu-id="74111-530">**Type**</span></span>                  | <span data-ttu-id="74111-531">Yes</span><span class="sxs-lookup"><span data-stu-id="74111-531">Yes</span></span>         | <span data-ttu-id="74111-532">Tipo della colonna corrispondente.</span><span class="sxs-lookup"><span data-stu-id="74111-532">The type of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="74111-533">**Ammette valori null**</span><span class="sxs-lookup"><span data-stu-id="74111-533">**Nullable**</span></span>              | <span data-ttu-id="74111-534">No</span><span class="sxs-lookup"><span data-stu-id="74111-534">No</span></span>          | <span data-ttu-id="74111-535">**True** (valore predefinito) o **False** a seconda del fatto che la colonna corrispondente può avere un valore null.</span><span class="sxs-lookup"><span data-stu-id="74111-535">**True** (the default value) or **False** depending on whether the corresponding column can have a null value.</span></span>                                                                                                                  |
| <span data-ttu-id="74111-536">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="74111-536">**DefaultValue**</span></span>          | <span data-ttu-id="74111-537">No</span><span class="sxs-lookup"><span data-stu-id="74111-537">No</span></span>          | <span data-ttu-id="74111-538">Valore predefinito della colonna corrispondente.</span><span class="sxs-lookup"><span data-stu-id="74111-538">The default value of the corresponding column.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="74111-539">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="74111-539">**MaxLength**</span></span>             | <span data-ttu-id="74111-540">No</span><span class="sxs-lookup"><span data-stu-id="74111-540">No</span></span>          | <span data-ttu-id="74111-541">Lunghezza massima della colonna corrispondente.</span><span class="sxs-lookup"><span data-stu-id="74111-541">The maximum length of the corresponding column.</span></span>                                                                                                                                                                                 |
| <span data-ttu-id="74111-542">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="74111-542">**FixedLength**</span></span>           | <span data-ttu-id="74111-543">No</span><span class="sxs-lookup"><span data-stu-id="74111-543">No</span></span>          | <span data-ttu-id="74111-544">**True** oppure **False** a seconda se il valore della colonna corrispondente essere archiviato come una stringa di lunghezza fissa.</span><span class="sxs-lookup"><span data-stu-id="74111-544">**True** or **False** depending on whether the corresponding column value will be stored as a fixed length string.</span></span>                                                                                                              |
| <span data-ttu-id="74111-545">**Precisione**</span><span class="sxs-lookup"><span data-stu-id="74111-545">**Precision**</span></span>             | <span data-ttu-id="74111-546">No</span><span class="sxs-lookup"><span data-stu-id="74111-546">No</span></span>          | <span data-ttu-id="74111-547">Precisione della colonna corrispondente.</span><span class="sxs-lookup"><span data-stu-id="74111-547">The precision of the corresponding column.</span></span>                                                                                                                                                                                      |
| <span data-ttu-id="74111-548">**Scala**</span><span class="sxs-lookup"><span data-stu-id="74111-548">**Scale**</span></span>                 | <span data-ttu-id="74111-549">No</span><span class="sxs-lookup"><span data-stu-id="74111-549">No</span></span>          | <span data-ttu-id="74111-550">Scala della colonna corrispondente.</span><span class="sxs-lookup"><span data-stu-id="74111-550">The scale of the corresponding column.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="74111-551">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="74111-551">**Unicode**</span></span>               | <span data-ttu-id="74111-552">No</span><span class="sxs-lookup"><span data-stu-id="74111-552">No</span></span>          | <span data-ttu-id="74111-553">**True** oppure **False** a seconda se il valore della colonna corrispondente essere archiviato come stringa Unicode.</span><span class="sxs-lookup"><span data-stu-id="74111-553">**True** or **False** depending on whether the corresponding column value will be stored as a Unicode string.</span></span>                                                                                                                   |
| <span data-ttu-id="74111-554">**regole di confronto**</span><span class="sxs-lookup"><span data-stu-id="74111-554">**Collation**</span></span>             | <span data-ttu-id="74111-555">No</span><span class="sxs-lookup"><span data-stu-id="74111-555">No</span></span>          | <span data-ttu-id="74111-556">Stringa che specifica la sequenza di confronto da utilizzare nell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="74111-556">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="74111-557">**SRID**</span><span class="sxs-lookup"><span data-stu-id="74111-557">**SRID**</span></span>                  | <span data-ttu-id="74111-558">No</span><span class="sxs-lookup"><span data-stu-id="74111-558">No</span></span>          | <span data-ttu-id="74111-559">Identificatore di riferimento spaziale del sistema.</span><span class="sxs-lookup"><span data-stu-id="74111-559">Spatial System Reference Identifier.</span></span> <span data-ttu-id="74111-560">Valido solo per le proprietà di tipi spaziali.</span><span class="sxs-lookup"><span data-stu-id="74111-560">Valid only for properties of spatial types.</span></span> <span data-ttu-id="74111-561">Per altre informazioni, vedere [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="74111-561">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="74111-562">**StoreGeneratedPattern**</span><span class="sxs-lookup"><span data-stu-id="74111-562">**StoreGeneratedPattern**</span></span> | <span data-ttu-id="74111-563">No</span><span class="sxs-lookup"><span data-stu-id="74111-563">No</span></span>          | <span data-ttu-id="74111-564">**None**, **Identity** (se il valore della colonna corrispondente è un'identità generata nel database) oppure **calcolata** (se il valore della colonna corrispondente viene calcolato nel database).</span><span class="sxs-lookup"><span data-stu-id="74111-564">**None**, **Identity** (if the corresponding column value is an identity that is generated in the database), or **Computed** (if the corresponding column value is computed in the database).</span></span> <span data-ttu-id="74111-565">Non valido per le proprietà di tipo di riga.</span><span class="sxs-lookup"><span data-stu-id="74111-565">Not Valid for RowType properties.</span></span> |

> [!NOTE]
> <span data-ttu-id="74111-566">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **proprietà** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-566">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="74111-567">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="74111-567">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="74111-568">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="74111-568">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="74111-569">Esempio</span><span class="sxs-lookup"><span data-stu-id="74111-569">Example</span></span>

<span data-ttu-id="74111-570">L'esempio seguente mostra un' **EntityType** elemento con l'elemento figlio due **proprietà** elementi:</span><span class="sxs-lookup"><span data-stu-id="74111-570">The following example shows an **EntityType** element with two child **Property** elements:</span></span>

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

## <a name="propertyref-element-ssdl"></a><span data-ttu-id="74111-571">Elemento PropertyRef (SSDL)</span><span class="sxs-lookup"><span data-stu-id="74111-571">PropertyRef Element (SSDL)</span></span>

<span data-ttu-id="74111-572">Il **PropertyRef** elemento schema store schema definition language (SSDL) fa riferimento a una proprietà definita in un elemento EntityType per indicare che la proprietà eseguirà uno dei seguenti ruoli:</span><span class="sxs-lookup"><span data-stu-id="74111-572">The **PropertyRef** element in store schema definition language (SSDL) references a property defined on an EntityType element to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="74111-573">Far parte della chiave primaria della tabella che la **EntityType** rappresenta.</span><span class="sxs-lookup"><span data-stu-id="74111-573">Be part of the primary key of the table that the **EntityType** represents.</span></span> <span data-ttu-id="74111-574">Uno o più **PropertyRef** elementi possono essere utilizzati per definire una chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="74111-574">One or more **PropertyRef** elements can be used to define a primary key.</span></span> <span data-ttu-id="74111-575">Per altre informazioni, vedere l'elemento Key.</span><span class="sxs-lookup"><span data-stu-id="74111-575">For more information, see Key element.</span></span>
-   <span data-ttu-id="74111-576">Rappresenterà l'entità finale dipendente o principale di un vincolo referenziale.</span><span class="sxs-lookup"><span data-stu-id="74111-576">Be the dependent or principal end of a referential constraint.</span></span> <span data-ttu-id="74111-577">Per altre informazioni, vedere ReferentialConstraint (elemento).</span><span class="sxs-lookup"><span data-stu-id="74111-577">For more information, see ReferentialConstraint element.</span></span>

<span data-ttu-id="74111-578">Il **PropertyRef** elemento può avere solo elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="74111-578">The **PropertyRef** element can only have the following child elements:</span></span>

-   <span data-ttu-id="74111-579">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="74111-579">Documentation (zero or one)</span></span>
-   <span data-ttu-id="74111-580">Elementi Annotation</span><span class="sxs-lookup"><span data-stu-id="74111-580">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="74111-581">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="74111-581">Applicable Attributes</span></span>

<span data-ttu-id="74111-582">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **PropertyRef** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-582">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="74111-583">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="74111-583">Attribute Name</span></span> | <span data-ttu-id="74111-584">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="74111-584">Is Required</span></span> | <span data-ttu-id="74111-585">Valore</span><span class="sxs-lookup"><span data-stu-id="74111-585">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="74111-586">**Name**</span><span class="sxs-lookup"><span data-stu-id="74111-586">**Name**</span></span>       | <span data-ttu-id="74111-587">Yes</span><span class="sxs-lookup"><span data-stu-id="74111-587">Yes</span></span>         | <span data-ttu-id="74111-588">Nome della proprietà alla quale viene fatto riferimento.</span><span class="sxs-lookup"><span data-stu-id="74111-588">The name of the referenced property.</span></span> |

> [!NOTE]
> <span data-ttu-id="74111-589">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **PropertyRef** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-589">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="74111-590">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="74111-590">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="74111-591">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="74111-591">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="74111-592">Esempio</span><span class="sxs-lookup"><span data-stu-id="74111-592">Example</span></span>

<span data-ttu-id="74111-593">L'esempio seguente mostra una **PropertyRef** elemento usato per definire una chiave primaria facendo riferimento a una proprietà definita in un **EntityType** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-593">The following example shows a **PropertyRef** element used to define a primary key by referencing a property that is defined on an **EntityType** element.</span></span>

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

## <a name="referentialconstraint-element-ssdl"></a><span data-ttu-id="74111-594">Elemento ReferentialConstraint (SSDL)</span><span class="sxs-lookup"><span data-stu-id="74111-594">ReferentialConstraint Element (SSDL)</span></span>

<span data-ttu-id="74111-595">Il **ReferentialConstraint** elemento schema store schema definition language (SSDL) rappresenta un vincolo di chiave esterna (chiamato anche vincolo referenziale) nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="74111-595">The **ReferentialConstraint** element in store schema definition language (SSDL) represents a foreign key constraint (also called a referential integrity constraint) in the underlying database.</span></span> <span data-ttu-id="74111-596">Le entità finali principale e dipendente del vincolo vengono specificate dagli elementi figlio principale e dipendente, rispettivamente.</span><span class="sxs-lookup"><span data-stu-id="74111-596">The principal and dependent ends of the constraint are specified by the Principal and Dependent child elements, respectively.</span></span> <span data-ttu-id="74111-597">Colonne che partecipano all'estremità principale e dipendente vengono fatto riferimento con gli elementi PropertyRef.</span><span class="sxs-lookup"><span data-stu-id="74111-597">Columns that participate in the principal and dependent ends are referenced with PropertyRef elements.</span></span>

<span data-ttu-id="74111-598">Il **ReferentialConstraint** è un elemento figlio facoltativo dell'elemento Association.</span><span class="sxs-lookup"><span data-stu-id="74111-598">The **ReferentialConstraint** element is an optional child element of the Association element.</span></span> <span data-ttu-id="74111-599">Se un **ReferentialConstraint** elemento non viene usato per eseguire il mapping il vincolo foreign key specificato nella **Association** elemento, un AssociationSetMapping elemento deve essere usato per eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="74111-599">If a **ReferentialConstraint** element is not used to map the foreign key constraint that is specified in the **Association** element, an AssociationSetMapping element must be used to do this.</span></span>

<span data-ttu-id="74111-600">Il **ReferentialConstraint** elemento può avere elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="74111-600">The **ReferentialConstraint** element can have the following child elements:</span></span>

-   <span data-ttu-id="74111-601">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="74111-601">Documentation (zero or one)</span></span>
-   <span data-ttu-id="74111-602">Entità (esattamente un elemento)</span><span class="sxs-lookup"><span data-stu-id="74111-602">Principal (exactly one)</span></span>
-   <span data-ttu-id="74111-603">Dipendente (esattamente un elemento)</span><span class="sxs-lookup"><span data-stu-id="74111-603">Dependent (exactly one)</span></span>
-   <span data-ttu-id="74111-604">Elementi Annotation (zero o più)</span><span class="sxs-lookup"><span data-stu-id="74111-604">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="74111-605">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="74111-605">Applicable Attributes</span></span>

<span data-ttu-id="74111-606">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **ReferentialConstraint** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-606">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferentialConstraint** element.</span></span> <span data-ttu-id="74111-607">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="74111-607">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="74111-608">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="74111-608">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="74111-609">Esempio</span><span class="sxs-lookup"><span data-stu-id="74111-609">Example</span></span>

<span data-ttu-id="74111-610">L'esempio seguente mostra un' **Association** elemento che utilizza un **ReferentialConstraint** elemento per specificare le colonne che fanno parte del **FK\_CustomerOrders**  vincolo di chiave esterna:</span><span class="sxs-lookup"><span data-stu-id="74111-610">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="returntype-element-ssdl"></a><span data-ttu-id="74111-611">Elemento ReturnType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="74111-611">ReturnType Element (SSDL)</span></span>

<span data-ttu-id="74111-612">Il **ReturnType** elemento di schema store schema definition language (SSDL) specifica il tipo restituito per una funzione che viene definito in un **funzione** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-612">The **ReturnType** element in store schema definition language (SSDL) specifies the return type for a function that is defined in a **Function** element.</span></span> <span data-ttu-id="74111-613">Una funzione di tipo restituito può essere specificato anche con un **ReturnType** attributo.</span><span class="sxs-lookup"><span data-stu-id="74111-613">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="74111-614">Il tipo restituito di una funzione viene specificato con il **tipo** attributo o il **ReturnType** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-614">The return type of a function is specified with the **Type** attribute or the **ReturnType** element.</span></span>

<span data-ttu-id="74111-615">Il **ReturnType** elemento può avere elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="74111-615">The **ReturnType** element can have the following child elements:</span></span>

- <span data-ttu-id="74111-616">Elemento CollectionType (uno)</span><span class="sxs-lookup"><span data-stu-id="74111-616">CollectionType (one)</span></span>  

> [!NOTE]
> <span data-ttu-id="74111-617">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **ReturnType** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-617">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** element.</span></span> <span data-ttu-id="74111-618">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="74111-618">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="74111-619">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="74111-619">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="74111-620">Esempio</span><span class="sxs-lookup"><span data-stu-id="74111-620">Example</span></span>

<span data-ttu-id="74111-621">L'esempio seguente usa un' **funzione** che restituisce una raccolta di righe.</span><span class="sxs-lookup"><span data-stu-id="74111-621">The following example uses a **Function** that returns a collection of rows.</span></span>

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


## <a name="rowtype-element-ssdl"></a><span data-ttu-id="74111-622">Elemento RowType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="74111-622">RowType Element (SSDL)</span></span>

<span data-ttu-id="74111-623">Oggetto **RowType** elemento schema store schema definition language (SSDL) definisce una struttura senza nome come reso tipo per una funzione definita nell'archivio.</span><span class="sxs-lookup"><span data-stu-id="74111-623">A **RowType** element in store schema definition language (SSDL) defines an unnamed structure as a return type for a function defined in the store.</span></span>

<span data-ttu-id="74111-624">Oggetto **RowType** è l'elemento figlio di **CollectionType** elemento:</span><span class="sxs-lookup"><span data-stu-id="74111-624">A **RowType** element is the child element of **CollectionType** element:</span></span>

<span data-ttu-id="74111-625">Oggetto **RowType** elemento può avere elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="74111-625">A **RowType** element can have the following child elements:</span></span>

- <span data-ttu-id="74111-626">Proprietà (una o più)</span><span class="sxs-lookup"><span data-stu-id="74111-626">Property (one or more)</span></span>  

### <a name="example"></a><span data-ttu-id="74111-627">Esempio</span><span class="sxs-lookup"><span data-stu-id="74111-627">Example</span></span>

<span data-ttu-id="74111-628">Nell'esempio seguente viene illustrata una funzione di archivio che usa un' **CollectionType** elemento per specificare che la funzione restituisce una raccolta di righe (come specificato nella **RowType** elemento).</span><span class="sxs-lookup"><span data-stu-id="74111-628">The following example shows a store function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>


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

## <a name="schema-element-ssdl"></a><span data-ttu-id="74111-629">Elemento Schema (SSDL)</span><span class="sxs-lookup"><span data-stu-id="74111-629">Schema Element (SSDL)</span></span>

<span data-ttu-id="74111-630">Il **Schema** elemento schema store schema definition language (SSDL) è l'elemento radice di una definizione di modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="74111-630">The **Schema** element in store schema definition language (SSDL) is the root element of a storage model definition.</span></span> <span data-ttu-id="74111-631">Contiene definizioni per gli oggetti, le funzioni e i contenitori che costituiscono un modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="74111-631">It contains definitions for the objects, functions, and containers that make up a storage model.</span></span>

<span data-ttu-id="74111-632">Il **Schema** elemento può contenere zero o più degli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="74111-632">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="74111-633">Associazione</span><span class="sxs-lookup"><span data-stu-id="74111-633">Association</span></span>
-   <span data-ttu-id="74111-634">EntityType</span><span class="sxs-lookup"><span data-stu-id="74111-634">EntityType</span></span>
-   <span data-ttu-id="74111-635">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="74111-635">EntityContainer</span></span>
-   <span data-ttu-id="74111-636">Funzione</span><span class="sxs-lookup"><span data-stu-id="74111-636">Function</span></span>

<span data-ttu-id="74111-637">Il **Schema** elemento utilizza le **Namespace** attributo per definire lo spazio dei nomi per gli oggetti di tipo e associazione di entità in un modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="74111-637">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type and association objects in a storage model.</span></span> <span data-ttu-id="74111-638">All'interno di uno spazio dei nomi due oggetti non possono avere lo stesso nome.</span><span class="sxs-lookup"><span data-stu-id="74111-638">Within a namespace, no two objects can have the same name.</span></span>

<span data-ttu-id="74111-639">Uno spazio dei nomi del modello di archiviazione è diverso dallo spazio dei nomi XML del **Schema** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-639">A storage model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="74111-640">Uno spazio dei nomi del modello di archiviazione (come definito per il **Namespace** attributi) è un contenitore logico per tipi di entità e tipi di associazione.</span><span class="sxs-lookup"><span data-stu-id="74111-640">A storage model namespace (as defined by the **Namespace** attribute) is a logical container for entity types and association types.</span></span> <span data-ttu-id="74111-641">Lo spazio dei nomi XML (indicato dal **xmlns** attributo) di un **Schema** elemento è lo spazio dei nomi predefinito per gli elementi figlio e attributi del **Schema** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-641">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="74111-642">Spazi dei nomi XML nel formato http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (dove YYYY e MM rappresentano un anno e mese rispettivamente) sono riservati a SSDL.</span><span class="sxs-lookup"><span data-stu-id="74111-642">XML namespaces of the form http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (where YYYY and MM represent a year and month respectively) are reserved for SSDL.</span></span> <span data-ttu-id="74111-643">Gli elementi e gli attributi personalizzati non possono essere in spazi dei nomi che hanno questo form.</span><span class="sxs-lookup"><span data-stu-id="74111-643">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="74111-644">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="74111-644">Applicable Attributes</span></span>

<span data-ttu-id="74111-645">Nella tabella seguente vengono descritti gli attributi possono essere applicati per il **Schema** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-645">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="74111-646">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="74111-646">Attribute Name</span></span>            | <span data-ttu-id="74111-647">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="74111-647">Is Required</span></span> | <span data-ttu-id="74111-648">Valore</span><span class="sxs-lookup"><span data-stu-id="74111-648">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="74111-649">**Spazio dei nomi**</span><span class="sxs-lookup"><span data-stu-id="74111-649">**Namespace**</span></span>             | <span data-ttu-id="74111-650">Yes</span><span class="sxs-lookup"><span data-stu-id="74111-650">Yes</span></span>         | <span data-ttu-id="74111-651">Spazio dei nomi del modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="74111-651">The namespace of the storage model.</span></span> <span data-ttu-id="74111-652">Il valore della **Namespace** viene usato in modo da formare il nome completo di un tipo.</span><span class="sxs-lookup"><span data-stu-id="74111-652">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="74111-653">Ad esempio, se un **EntityType** denominata *cliente* è lo spazio dei nomi examplemodel. Store, quindi specificare il nome completo del **EntityType** è Examplemodel.</span><span class="sxs-lookup"><span data-stu-id="74111-653">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace, then the fully qualified name of the **EntityType** is ExampleModel.Store.Customer.</span></span> <br/> <span data-ttu-id="74111-654">Le stringhe seguenti non possono essere usate come valore per il **Namespace** attributo: **System**, **temporaneo**, oppure **Edm**.</span><span class="sxs-lookup"><span data-stu-id="74111-654">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="74111-655">Il valore per il **Namespace** attributo non può essere identico al valore per il **Namespace** attributo nell'elemento Schema CSDL.</span><span class="sxs-lookup"><span data-stu-id="74111-655">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the CSDL Schema element.</span></span> |
| <span data-ttu-id="74111-656">**Alias**</span><span class="sxs-lookup"><span data-stu-id="74111-656">**Alias**</span></span>                 | <span data-ttu-id="74111-657">No</span><span class="sxs-lookup"><span data-stu-id="74111-657">No</span></span>          | <span data-ttu-id="74111-658">Identificatore utilizzato al posto del nome dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="74111-658">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="74111-659">Ad esempio, se un **EntityType** denominata *Customer* è nello spazio dei nomi examplemodel. Store e il valore del **Alias** attributo è *StorageModel*, è possibile utilizzare storagemodel. Customer come nome completo del **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="74111-659">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace and the value of the **Alias** attribute is *StorageModel*, then you can use StorageModel.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="74111-660">**Provider**</span><span class="sxs-lookup"><span data-stu-id="74111-660">**Provider**</span></span>              | <span data-ttu-id="74111-661">Yes</span><span class="sxs-lookup"><span data-stu-id="74111-661">Yes</span></span>         | <span data-ttu-id="74111-662">Provider di dati.</span><span class="sxs-lookup"><span data-stu-id="74111-662">The data provider.</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="74111-663">**ProviderManifestToken**</span><span class="sxs-lookup"><span data-stu-id="74111-663">**ProviderManifestToken**</span></span> | <span data-ttu-id="74111-664">Yes</span><span class="sxs-lookup"><span data-stu-id="74111-664">Yes</span></span>         | <span data-ttu-id="74111-665">Token che indica al provider quale manifesto del provider restituire.</span><span class="sxs-lookup"><span data-stu-id="74111-665">A token that indicates to the provider which provider manifest to return.</span></span> <span data-ttu-id="74111-666">Non è definito alcun formato per il token.</span><span class="sxs-lookup"><span data-stu-id="74111-666">No format for the token is defined.</span></span> <span data-ttu-id="74111-667">I valori per il token sono definiti dal provider.</span><span class="sxs-lookup"><span data-stu-id="74111-667">Values for the token are defined by the provider.</span></span> <span data-ttu-id="74111-668">Per informazioni sui token del manifesto del provider SQL Server, vedere SqlClient per Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="74111-668">For information about SQL Server provider manifest tokens, see SqlClient for Entity Framework.</span></span>                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a><span data-ttu-id="74111-669">Esempio</span><span class="sxs-lookup"><span data-stu-id="74111-669">Example</span></span>

<span data-ttu-id="74111-670">L'esempio seguente mostra una **Schema** elemento che contiene un' **EntityContainer** elemento, due **EntityType** elementi e uno **associazione** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-670">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

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

## <a name="annotation-attributes"></a><span data-ttu-id="74111-671">Attributi di annotazione</span><span class="sxs-lookup"><span data-stu-id="74111-671">Annotation Attributes</span></span>

<span data-ttu-id="74111-672">Gli attributi di annotazione in Store Schema Definition Language (SSDL) sono attributi XML personalizzati nel modello di archiviazione che forniscono metadati aggiuntivi sugli elementi del modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="74111-672">Annotation attributes in store schema definition language (SSDL) are custom XML attributes in the storage model that provide extra metadata about the elements in the storage model.</span></span> <span data-ttu-id="74111-673">Oltre ad avere una struttura XML valida, agli attributi di annotazione si applicano i vincoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="74111-673">In addition to having valid XML structure, the following constraints apply to annotation attributes:</span></span>

-   <span data-ttu-id="74111-674">Gli attributi di annotazione non devono trovarsi in spazi dei nomi XML riservati a SSDL.</span><span class="sxs-lookup"><span data-stu-id="74111-674">Annotation attributes must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="74111-675">I nomi completi di due attributi di annotazione non devono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="74111-675">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="74111-676">È possibile applicare più attributi di annotazione a un determinato elemento SSDL.</span><span class="sxs-lookup"><span data-stu-id="74111-676">More than one annotation attribute may be applied to a given SSDL element.</span></span> <span data-ttu-id="74111-677">Metadati contenuti negli elementi di annotazione sono accessibili in fase di esecuzione usando le classi nello spazio dei nomi System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="74111-677">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="74111-678">Esempio</span><span class="sxs-lookup"><span data-stu-id="74111-678">Example</span></span>

<span data-ttu-id="74111-679">L'esempio seguente illustra un elemento EntityType che presenta un attributo di annotazione applicato per il **OrderId** proprietà.</span><span class="sxs-lookup"><span data-stu-id="74111-679">The following example shows an EntityType element that has an annotation attribute applied to the **OrderId** property.</span></span> <span data-ttu-id="74111-680">L'esempio illustra anche un elemento di annotazione aggiunto per il **EntityType** elemento.</span><span class="sxs-lookup"><span data-stu-id="74111-680">The example also show an annotation element added to the **EntityType** element.</span></span>

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

## <a name="annotation-elements-ssdl"></a><span data-ttu-id="74111-681">Elementi Annotation (SSDL)</span><span class="sxs-lookup"><span data-stu-id="74111-681">Annotation Elements (SSDL)</span></span>

<span data-ttu-id="74111-682">Gli elementi Annotation in SSDL (Store Schema Definition Language) sono elementi XML personalizzati nel modello di archiviazione che forniscono metadati aggiuntivi sul modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="74111-682">Annotation elements in store schema definition language (SSDL) are custom XML elements in the storage model that provide extra metadata about the storage model.</span></span> <span data-ttu-id="74111-683">Oltre ad avere una struttura XML valida, agli elementi Annotation si applicano i vincoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="74111-683">In addition to having valid XML structure, the following constraints apply to annotation elements:</span></span>

-   <span data-ttu-id="74111-684">Gli elementi Annotation non devono trovarsi in spazi dei nomi XML riservati a SSDL.</span><span class="sxs-lookup"><span data-stu-id="74111-684">Annotation elements must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="74111-685">I nomi completi di due elementi Annotation non devono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="74111-685">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="74111-686">Gli elementi Annotation devono apparire dopo tutti gli altri elementi figlio di un dato elemento SSDL.</span><span class="sxs-lookup"><span data-stu-id="74111-686">Annotation elements must appear after all other child elements of a given SSDL element.</span></span>

<span data-ttu-id="74111-687">Più elementi Annotation possono essere figli di un dato elemento SSDL.</span><span class="sxs-lookup"><span data-stu-id="74111-687">More than one annotation element may be a child of a given SSDL element.</span></span> <span data-ttu-id="74111-688">A partire da .NET Framework versione 4, i metadati contenuti in elementi annotation sono accessibile in fase di esecuzione usando le classi nello spazio dei nomi System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="74111-688">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="74111-689">Esempio</span><span class="sxs-lookup"><span data-stu-id="74111-689">Example</span></span>

<span data-ttu-id="74111-690">L'esempio seguente illustra un elemento EntityType contenente un elemento annotation (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="74111-690">The following example shows an EntityType element that has an annotation element (**CustomElement**).</span></span> <span data-ttu-id="74111-691">L'esempio mostra anche un attributo di annotazione applicato per il **OrderId** proprietà.</span><span class="sxs-lookup"><span data-stu-id="74111-691">The example also shows an annotation attribute applied to the **OrderId** property.</span></span>

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

## <a name="facets-ssdl"></a><span data-ttu-id="74111-692">Facet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="74111-692">Facets (SSDL)</span></span>

<span data-ttu-id="74111-693">I facet in schema store schema definition language (SSDL) rappresentano vincoli sui tipi di colonna sono specificati negli elementi proprietà.</span><span class="sxs-lookup"><span data-stu-id="74111-693">Facets in store schema definition language (SSDL) represent constraints on column types that are specified in Property elements.</span></span> <span data-ttu-id="74111-694">I facet vengono implementati come attributi XML sul **proprietà** elementi.</span><span class="sxs-lookup"><span data-stu-id="74111-694">Facets are implemented as XML attributes on **Property** elements.</span></span>

<span data-ttu-id="74111-695">Nella tabella seguente vengono descritti i facet supportati in SSDL:</span><span class="sxs-lookup"><span data-stu-id="74111-695">The following table describes the facets that are supported in SSDL:</span></span>

| <span data-ttu-id="74111-696">Facet</span><span class="sxs-lookup"><span data-stu-id="74111-696">Facet</span></span>           | <span data-ttu-id="74111-697">Descrizione</span><span class="sxs-lookup"><span data-stu-id="74111-697">Description</span></span>                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="74111-698">**regole di confronto**</span><span class="sxs-lookup"><span data-stu-id="74111-698">**Collation**</span></span>   | <span data-ttu-id="74111-699">Specifica la sequenza di ordinamento da usare quando si eseguono operazioni di confronto e di ordinamento su valori della proprietà.</span><span class="sxs-lookup"><span data-stu-id="74111-699">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                             |
| <span data-ttu-id="74111-700">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="74111-700">**FixedLength**</span></span> | <span data-ttu-id="74111-701">Specifica se la lunghezza del valore della colonna può variare.</span><span class="sxs-lookup"><span data-stu-id="74111-701">Specifies whether the length of the column value can vary.</span></span>                                                                                                                                                                                                  |
| <span data-ttu-id="74111-702">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="74111-702">**MaxLength**</span></span>   | <span data-ttu-id="74111-703">Specifica la lunghezza massima del valore della colonna.</span><span class="sxs-lookup"><span data-stu-id="74111-703">Specifies the maximum length of the column value.</span></span>                                                                                                                                                                                                           |
| <span data-ttu-id="74111-704">**Precisione**</span><span class="sxs-lookup"><span data-stu-id="74111-704">**Precision**</span></span>   | <span data-ttu-id="74111-705">Per le proprietà di tipo **decimale**, specifica il numero di cifre può avere un valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="74111-705">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="74111-706">Per le proprietà di tipo **tempo**, **DateTime**, e **DateTimeOffset**, specifica il numero di cifre per la parte frazionaria di secondi del valore della colonna.</span><span class="sxs-lookup"><span data-stu-id="74111-706">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the column value.</span></span> |
| <span data-ttu-id="74111-707">**Scala**</span><span class="sxs-lookup"><span data-stu-id="74111-707">**Scale**</span></span>       | <span data-ttu-id="74111-708">Specifica il numero di cifre a destra del separatore decimale per il valore della colonna.</span><span class="sxs-lookup"><span data-stu-id="74111-708">Specifies the number of digits to the right of the decimal point for the column value.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="74111-709">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="74111-709">**Unicode**</span></span>     | <span data-ttu-id="74111-710">Indica se il valore della colonna viene archiviato come Unicode.</span><span class="sxs-lookup"><span data-stu-id="74111-710">Indicates whether the column value is stored as Unicode.</span></span>                                                                                                                                                                                                    |
