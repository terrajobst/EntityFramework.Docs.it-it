---
title: Specifica CSDL-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
ms.openlocfilehash: 642e5977ecbbf0c474cac1ceae19d33a135aa875
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182590"
---
# <a name="csdl-specification"></a><span data-ttu-id="cb8fc-102">Specifica CSDL</span><span class="sxs-lookup"><span data-stu-id="cb8fc-102">CSDL Specification</span></span>
<span data-ttu-id="cb8fc-103">Conceptual Schema Definition Language (CSDL) è un linguaggio basato su XML che descrive le entità, le relazioni e le funzioni che costituiscono un modello concettuale di un'applicazione basata sui dati.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-103">Conceptual schema definition language (CSDL) is an XML-based language that describes the entities, relationships, and functions that make up a conceptual model of a data-driven application.</span></span> <span data-ttu-id="cb8fc-104">Questo modello concettuale può essere utilizzato dal Entity Framework o WCF Data Services.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-104">This conceptual model can be used by the Entity Framework or WCF Data Services.</span></span> <span data-ttu-id="cb8fc-105">I metadati descritti con CSDL vengono utilizzati dalla Entity Framework per eseguire il mapping di entità e relazioni definite in un modello concettuale a un'origine dati.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-105">The metadata that is described with CSDL is used by the Entity Framework to map entities and relationships that are defined in a conceptual model to a data source.</span></span> <span data-ttu-id="cb8fc-106">Per ulteriori informazioni, vedere [specifica SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) e [specifica MSL](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-106">For more information, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) and [MSL Specification](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span></span>

<span data-ttu-id="cb8fc-107">CSDL è l'implementazione Entity Framework dell'Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-107">CSDL is the Entity Framework's implementation of the Entity Data Model.</span></span>

<span data-ttu-id="cb8fc-108">In un'applicazione Entity Framework, i metadati del modello concettuale vengono caricati da un file con estensione CSDL (scritto in CSDL) in un'istanza di System. Data. Metadata. Edm. EdmItemCollection ed è possibile accedervi tramite metodi nel Classe System. Data. Metadata. Edm. MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-108">In an Entity Framework application, conceptual model metadata is loaded from a .csdl file (written in CSDL) into an instance of the System.Data.Metadata.Edm.EdmItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="cb8fc-109">Entity Framework usa i metadati del modello concettuale per tradurre le query sul modello concettuale in comandi specifici dell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-109">Entity Framework uses conceptual model metadata to translate queries against the conceptual model to data source-specific commands.</span></span>

<span data-ttu-id="cb8fc-110">La finestra di progettazione EF archivia le informazioni sul modello concettuale in un file con estensione edmx in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-110">The EF Designer stores conceptual model information in an .edmx file at design time.</span></span> <span data-ttu-id="cb8fc-111">In fase di compilazione, la finestra di progettazione EF utilizza le informazioni in un file con estensione edmx per creare il file con estensione csdl necessario per Entity Framework in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-111">At build time, the EF Designer uses information in an .edmx file to create the .csdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="cb8fc-112">Le versioni di CSDL si differenziano tra loro per gli spazi dei nomi XML.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-112">Versions of CSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="cb8fc-113">Versione CSDL</span><span class="sxs-lookup"><span data-stu-id="cb8fc-113">CSDL Version</span></span> | <span data-ttu-id="cb8fc-114">Spazio dei nomi XML</span><span class="sxs-lookup"><span data-stu-id="cb8fc-114">XML Namespace</span></span>                                |
|:-------------|:---------------------------------------------|
| <span data-ttu-id="cb8fc-115">CSDL V1</span><span class="sxs-lookup"><span data-stu-id="cb8fc-115">CSDL v1</span></span>      | https://schemas.microsoft.com/ado/2006/04/edm |
| <span data-ttu-id="cb8fc-116">CSDL V2</span><span class="sxs-lookup"><span data-stu-id="cb8fc-116">CSDL v2</span></span>      | https://schemas.microsoft.com/ado/2008/09/edm |
| <span data-ttu-id="cb8fc-117">CSDL V3</span><span class="sxs-lookup"><span data-stu-id="cb8fc-117">CSDL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a><span data-ttu-id="cb8fc-118">Elemento Association (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-118">Association Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-119">Un elemento **Association** definisce una relazione tra due tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-119">An **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="cb8fc-120">Un'associazione deve specificare i tipi di entità coinvolti nella relazione e il possibile numero di tipi di entità a ogni entità finale della relazione, che è noto come molteplicità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-120">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="cb8fc-121">La molteplicità di un'entità finale di un'associazione può avere un valore pari a uno (1), zero o uno (0.. 1) o molti (\*).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-121">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="cb8fc-122">Queste informazioni vengono specificate in due elementi End figlio.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-122">This information is specified in two child End elements.</span></span>

<span data-ttu-id="cb8fc-123">Le istanze del tipo di entità in corrispondenza di un'entità finale di un'associazione sono accessibili attraverso proprietà di navigazione o chiavi esterne se sono esposte in un tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-123">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys, if they are exposed on an entity type.</span></span>

<span data-ttu-id="cb8fc-124">In un'applicazione, un'istanza di un'associazione rappresenta un'associazione specifica tra istanze di tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-124">In an application, an instance of an association represents a specific association between instances of entity types.</span></span> <span data-ttu-id="cb8fc-125">Le istanze dell'associazione sono raggruppate logicamente in un set di associazioni.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-125">Association instances are logically grouped in an association set.</span></span>

<span data-ttu-id="cb8fc-126">Un elemento **Association** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="cb8fc-126">An **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cb8fc-127">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-127">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="cb8fc-128">End (esattamente 2 elementi)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-128">End (exactly 2 elements)</span></span>
-   <span data-ttu-id="cb8fc-129">ReferentialConstraint (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-129">ReferentialConstraint (zero or one element)</span></span>
-   <span data-ttu-id="cb8fc-130">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-130">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-131">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-131">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-132">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-132">The table below describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="cb8fc-133">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="cb8fc-133">Attribute Name</span></span> | <span data-ttu-id="cb8fc-134">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-134">Is Required</span></span> | <span data-ttu-id="cb8fc-135">Value</span><span class="sxs-lookup"><span data-stu-id="cb8fc-135">Value</span></span>                        |
|:---------------|:------------|:-----------------------------|
| <span data-ttu-id="cb8fc-136">**Name**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-136">**Name**</span></span>       | <span data-ttu-id="cb8fc-137">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-137">Yes</span></span>         | <span data-ttu-id="cb8fc-138">Nome dell'associazione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-138">The name of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="cb8fc-139">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-139">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="cb8fc-140">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-140">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-141">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-141">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="cb8fc-142">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-142">Example</span></span>

<span data-ttu-id="cb8fc-143">Nell'esempio seguente viene illustrato un elemento **Association** che definisce l'associazione **CustomerOrders** quando le chiavi esterne non sono state esposte sui tipi di entità **Customer** e **Order** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-143">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have not been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="cb8fc-144">I valori di **molteplicità** per ogni **estremità** dell'associazione indicano che molti **ordini** possono essere associati a un **cliente**, ma solo un **cliente** può essere associato a un **ordine**.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-144">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="cb8fc-145">Inoltre, l'elemento **OnDelete** indica che tutti **gli ordini** relativi a un determinato **cliente** e che sono stati caricati in ObjectContext verranno eliminati se il **cliente** viene eliminato.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-145">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

<span data-ttu-id="cb8fc-146">Nell'esempio seguente viene illustrato un elemento **Association** che definisce l'associazione **CustomerOrders** quando le chiavi esterne sono state esposte sui tipi di entità **Customer** e **Order** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-146">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="cb8fc-147">Con le chiavi esterne esposte, la relazione tra le entità viene gestita con un elemento **ReferentialConstraint** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-147">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element.</span></span> <span data-ttu-id="cb8fc-148">Non è necessario un elemento AssociationSetMapping corrispondente per eseguire il mapping dell'associazione all'origine dati.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-148">A corresponding AssociationSetMapping element is not necessary to map this association to the data source.</span></span>

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
 

 

## <a name="associationset-element-csdl"></a><span data-ttu-id="cb8fc-149">Elemento AssociationSet (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-149">AssociationSet Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-150">L'elemento **associativo** in Conceptual Schema Definition Language (CSDL) è un contenitore logico per le istanze di associazione dello stesso tipo.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-150">The **AssociationSet** element in conceptual schema definition language (CSDL) is a logical container for association instances of the same type.</span></span> <span data-ttu-id="cb8fc-151">Un set di associazioni fornisce una definizione per il raggruppamento di istanze di associazioni in modo che possano essere mappate a un'origine dati.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-151">An association set provides a definition for grouping association instances so that they can be mapped to a data source.</span></span>  

<span data-ttu-id="cb8fc-152">L'elemento **associationname** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="cb8fc-152">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cb8fc-153">Documentazione (zero o un elemento consentito)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-153">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="cb8fc-154">End (esattamente due elementi necessari)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-154">End (exactly two elements required)</span></span>
-   <span data-ttu-id="cb8fc-155">Elementi Annotation (zero o più elementi consentiti)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-155">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="cb8fc-156">L'attributo **Association** specifica il tipo di associazione contenuto in un set di associazioni.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-156">The **Association** attribute specifies the type of association that an association set contains.</span></span> <span data-ttu-id="cb8fc-157">I set di entità che costituiscono le entità finali di un set di associazioni vengono specificati con esattamente due elementi **end** figlio.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-157">The entity sets that make up the ends of an association set are specified with exactly two child **End** elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-158">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-158">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-159">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento di **associazione** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-159">The table below describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="cb8fc-160">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="cb8fc-160">Attribute Name</span></span>  | <span data-ttu-id="cb8fc-161">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-161">Is Required</span></span> | <span data-ttu-id="cb8fc-162">Value</span><span class="sxs-lookup"><span data-stu-id="cb8fc-162">Value</span></span>                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cb8fc-163">**Name**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-163">**Name**</span></span>        | <span data-ttu-id="cb8fc-164">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-164">Yes</span></span>         | <span data-ttu-id="cb8fc-165">Nome del set di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-165">The name of the entity set.</span></span> <span data-ttu-id="cb8fc-166">Il valore dell'attributo **Name** non può corrispondere al valore dell'attributo **Association** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-166">The value of the **Name** attribute cannot be the same as the value of the **Association** attribute.</span></span>                                 |
| <span data-ttu-id="cb8fc-167">**Associazione**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-167">**Association**</span></span> | <span data-ttu-id="cb8fc-168">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-168">Yes</span></span>         | <span data-ttu-id="cb8fc-169">Il nome completo dell'associazione le cui istanze sono contenute nel set di associazioni.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-169">The fully-qualified name of the association that the association set contains instances of.</span></span> <span data-ttu-id="cb8fc-170">L'associazione deve essere nello stesso spazio dei nomi del set di associazioni.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-170">The association must be in the same namespace as the association set.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="cb8fc-171">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento di **associazione** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-171">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="cb8fc-172">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-172">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-173">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-173">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="cb8fc-174">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-174">Example</span></span>

<span data-ttu-id="cb8fc-175">Nell'esempio seguente viene illustrato un elemento **EntityContainer** con due elementi di **associazione** :</span><span class="sxs-lookup"><span data-stu-id="cb8fc-175">The following example shows an **EntityContainer** element with two **AssociationSet** elements:</span></span>

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
 

 

## <a name="collectiontype-element-csdl"></a><span data-ttu-id="cb8fc-176">Elemento CollectionType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-176">CollectionType Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-177">L'elemento **CollectionType** in Conceptual Schema Definition Language (CSDL) specifica che un parametro di funzione o un tipo restituito della funzione è una raccolta.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-177">The **CollectionType** element in conceptual schema definition language (CSDL) specifies that a function parameter or function return type is a collection.</span></span> <span data-ttu-id="cb8fc-178">L'elemento **CollectionType** può essere un elemento figlio dell'elemento Parameter o dell'elemento ReturnType (Function).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-178">The **CollectionType** element can be a child of the Parameter element or the ReturnType (Function) element.</span></span> <span data-ttu-id="cb8fc-179">Il tipo di raccolta può essere specificato tramite l'attributo **Type** o uno degli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="cb8fc-179">The type of collection can be specified by using either the **Type** attribute or one of the following child elements:</span></span>

-   <span data-ttu-id="cb8fc-180">**CollectionType**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-180">**CollectionType**</span></span>
-   <span data-ttu-id="cb8fc-181">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="cb8fc-181">ReferenceType</span></span>
-   <span data-ttu-id="cb8fc-182">RowType</span><span class="sxs-lookup"><span data-stu-id="cb8fc-182">RowType</span></span>
-   <span data-ttu-id="cb8fc-183">TypeRef</span><span class="sxs-lookup"><span data-stu-id="cb8fc-183">TypeRef</span></span>

> [!NOTE]
> <span data-ttu-id="cb8fc-184">Un modello non convaliderà se il tipo di una raccolta viene specificato sia con l'attributo di **tipo** che con un elemento figlio.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-184">A model will not validate if the type of a collection is specified with both the **Type** attribute and a child element.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-185">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-185">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-186">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **CollectionType** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-186">The following table describes the attributes that can be applied to the **CollectionType** element.</span></span> <span data-ttu-id="cb8fc-187">Si noti che gli attributi **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **scale**, **Unicode**e **Collation** sono applicabili solo alle raccolte di **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-187">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to collections of **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="cb8fc-188">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="cb8fc-188">Attribute Name</span></span>                                                          | <span data-ttu-id="cb8fc-189">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-189">Is Required</span></span> | <span data-ttu-id="cb8fc-190">Value</span><span class="sxs-lookup"><span data-stu-id="cb8fc-190">Value</span></span>                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cb8fc-191">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-191">**Type**</span></span>                                                                | <span data-ttu-id="cb8fc-192">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-192">No</span></span>          | <span data-ttu-id="cb8fc-193">Tipo della raccolta.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-193">The type of the collection.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="cb8fc-194">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-194">**Nullable**</span></span>                                                            | <span data-ttu-id="cb8fc-195">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-195">No</span></span>          | <span data-ttu-id="cb8fc-196">**True** (valore predefinito) o **false** a seconda che la proprietà possa avere un valore null.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-196">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                 |
| <span data-ttu-id="cb8fc-197">> In CSDL V1, una proprietà di tipo complesso deve avere `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-197">> In the CSDL v1, a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                  |
| <span data-ttu-id="cb8fc-198">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-198">**DefaultValue**</span></span>                                                        | <span data-ttu-id="cb8fc-199">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-199">No</span></span>          | <span data-ttu-id="cb8fc-200">Valore predefinito della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-200">The default value of the property.</span></span>                                                                                                                                                                                               |
| <span data-ttu-id="cb8fc-201">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-201">**MaxLength**</span></span>                                                           | <span data-ttu-id="cb8fc-202">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-202">No</span></span>          | <span data-ttu-id="cb8fc-203">Lunghezza massima del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-203">The maximum length of the property value.</span></span>                                                                                                                                                                                        |
| <span data-ttu-id="cb8fc-204">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-204">**FixedLength**</span></span>                                                         | <span data-ttu-id="cb8fc-205">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-205">No</span></span>          | <span data-ttu-id="cb8fc-206">**True** o **false** a seconda che il valore della proprietà venga archiviato come stringa a lunghezza fissa.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-206">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                           |
| <span data-ttu-id="cb8fc-207">**Precisione**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-207">**Precision**</span></span>                                                           | <span data-ttu-id="cb8fc-208">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-208">No</span></span>          | <span data-ttu-id="cb8fc-209">Precisione del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-209">The precision of the property value.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="cb8fc-210">**Scala**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-210">**Scale**</span></span>                                                               | <span data-ttu-id="cb8fc-211">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-211">No</span></span>          | <span data-ttu-id="cb8fc-212">Scala del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-212">The scale of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="cb8fc-213">**SRID**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-213">**SRID**</span></span>                                                                | <span data-ttu-id="cb8fc-214">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-214">No</span></span>          | <span data-ttu-id="cb8fc-215">Identificatore di riferimento del sistema spaziale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-215">Spatial System Reference Identifier.</span></span> <span data-ttu-id="cb8fc-216">Valido solo per le proprietà dei tipi spaziali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-216">Valid only for properties of spatial types.</span></span><span data-ttu-id="cb8fc-217">   Per ulteriori informazioni, vedere [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-217">   For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span></span> |
| <span data-ttu-id="cb8fc-218">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-218">**Unicode**</span></span>                                                             | <span data-ttu-id="cb8fc-219">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-219">No</span></span>          | <span data-ttu-id="cb8fc-220">**True** o **false** a seconda che il valore della proprietà venga archiviato come stringa Unicode.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-220">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                                |
| <span data-ttu-id="cb8fc-221">**Confronto**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-221">**Collation**</span></span>                                                           | <span data-ttu-id="cb8fc-222">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-222">No</span></span>          | <span data-ttu-id="cb8fc-223">Stringa che specifica la sequenza di confronto da utilizzare nell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-223">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                    |

 

> [!NOTE]
> <span data-ttu-id="cb8fc-224">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **CollectionType** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-224">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="cb8fc-225">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-226">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="cb8fc-227">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-227">Example</span></span>

<span data-ttu-id="cb8fc-228">Nell'esempio seguente viene illustrata una funzione definita dal modello che utilizza un elemento **CollectionType** per specificare che la funzione restituisce una raccolta di tipi di entità **Person** (come specificato con l'attributo **elementType** ).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-228">The following example shows a model-defined function that that uses a **CollectionType** element to specify that the function returns a collection of **Person** entity types (as specified with the **ElementType** attribute).</span></span>

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
 

<span data-ttu-id="cb8fc-229">Nell'esempio seguente viene illustrata una funzione definita dal modello che utilizza un elemento **CollectionType** per specificare che la funzione restituisce una raccolta di righe, come specificato nell'elemento **RowType** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-229">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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
 

<span data-ttu-id="cb8fc-230">Nell'esempio seguente viene illustrata una funzione definita dal modello che utilizza l'elemento **CollectionType** per specificare che la funzione accetta come parametro una raccolta di tipi di entità **Department** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-230">The following example shows a model-defined function that uses the **CollectionType** element to specify that the function accepts as a parameter a collection of **Department** entity types.</span></span>

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
 

 

## <a name="complextype-element-csdl"></a><span data-ttu-id="cb8fc-231">Elemento ComplexType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-231">ComplexType Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-232">Un elemento **complexType** definisce una struttura di dati composta da proprietà **EDMSimpleType** o altri tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-232">A **ComplexType** element defines a data structure composed of **EdmSimpleType** properties or other complex types.</span></span><span data-ttu-id="cb8fc-233">  Un tipo complesso può essere una proprietà di un tipo di entità o un altro tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-233">  A complex type can be a property of an entity type or another complex type.</span></span> <span data-ttu-id="cb8fc-234">Un tipo complesso è simile a un tipo di entità in quanto il tipo complesso definisce i dati.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-234">A complex type is similar to an entity type in that a complex type defines data.</span></span> <span data-ttu-id="cb8fc-235">Tuttavia esistono alcune differenze importanti tra i tipi complessi e i tipi di entità:</span><span class="sxs-lookup"><span data-stu-id="cb8fc-235">However, there are some key differences between complex types and entity types:</span></span>

-   <span data-ttu-id="cb8fc-236">I tipi complessi non dispongono di identità o chiavi e pertanto non possono esistere indipendentemente.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-236">Complex types do not have identities (or keys) and therefore cannot exist independently.</span></span> <span data-ttu-id="cb8fc-237">I tipi complessi possono esistere solo come proprietà di tipi di entità o gli altri tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-237">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="cb8fc-238">I tipi complessi non possono partecipare alle associazioni.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-238">Complex types cannot participate in associations.</span></span> <span data-ttu-id="cb8fc-239">Nessuna entità finale di un'associazione può essere un tipo complesso e pertanto non è possibile definire le proprietà di navigazione per i tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-239">Neither end of an association can be a complex type, and therefore navigation properties cannot be defined for complex types.</span></span>
-   <span data-ttu-id="cb8fc-240">Una proprietà di tipo complesso non può avere un valore null, sebbene ogni proprietà scalare di un tipo complesso possa essere impostata su Null.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-240">A complex type property cannot have a null value, though the scalar properties of a complex type may each be set to null.</span></span>

<span data-ttu-id="cb8fc-241">Un elemento **complexType** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="cb8fc-241">A **ComplexType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cb8fc-242">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-242">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="cb8fc-243">Property (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-243">Property (zero or more elements)</span></span>
-   <span data-ttu-id="cb8fc-244">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-244">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="cb8fc-245">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **complexType** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-245">The table below describes the attributes that can be applied to the **ComplexType** element.</span></span>

| <span data-ttu-id="cb8fc-246">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="cb8fc-246">Attribute Name</span></span>                                                                                                 | <span data-ttu-id="cb8fc-247">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-247">Is Required</span></span> | <span data-ttu-id="cb8fc-248">Value</span><span class="sxs-lookup"><span data-stu-id="cb8fc-248">Value</span></span>                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cb8fc-249">nome</span><span class="sxs-lookup"><span data-stu-id="cb8fc-249">Name</span></span>                                                                                                           | <span data-ttu-id="cb8fc-250">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-250">Yes</span></span>         | <span data-ttu-id="cb8fc-251">Nome del tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-251">The name of the complex type.</span></span> <span data-ttu-id="cb8fc-252">Il nome di un tipo complesso non può essere uguale a quello di un altro tipo complesso, di un tipo di entità o di un'associazione che si trova entro l'ambito del modello.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-252">The name of a complex type cannot be the same as the name of another complex type, entity type, or association that is within the scope of the model.</span></span> |
| <span data-ttu-id="cb8fc-253">BaseType</span><span class="sxs-lookup"><span data-stu-id="cb8fc-253">BaseType</span></span>                                                                                                       | <span data-ttu-id="cb8fc-254">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-254">No</span></span>          | <span data-ttu-id="cb8fc-255">Nome di un altro tipo complesso che è il tipo di base del tipo complesso definito.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-255">The name of another complex type that is the base type of the complex type that is being defined.</span></span> <br/> [!NOTE]                                                                     |
| <span data-ttu-id="cb8fc-256">> Questo attributo non è applicabile in CSDL V1.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-256">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="cb8fc-257">L'ereditarietà per i tipi complessi non è supportata in quella versione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-257">Inheritance for complex types is not supported in that version.</span></span> |             |                                                                                                                                                                                     |
| <span data-ttu-id="cb8fc-258">Astratta</span><span class="sxs-lookup"><span data-stu-id="cb8fc-258">Abstract</span></span>                                                                                                       | <span data-ttu-id="cb8fc-259">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-259">No</span></span>          | <span data-ttu-id="cb8fc-260">**True** o **false** (valore predefinito) a seconda che il tipo complesso sia un tipo astratto.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-260">**True** or **False** (the default value) depending on whether the complex type is an abstract type.</span></span> <br/> [!NOTE]                                                                  |
| <span data-ttu-id="cb8fc-261">> Questo attributo non è applicabile in CSDL V1.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-261">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="cb8fc-262">I tipi complessi in quella versione non possono essere tipi astratti.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-262">Complex types in that version cannot be abstract types.</span></span>         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="cb8fc-263">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **complexType** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-263">Any number of annotation attributes (custom XML attributes) may be applied to the **ComplexType** element.</span></span> <span data-ttu-id="cb8fc-264">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-264">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-265">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-265">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="cb8fc-266">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-266">Example</span></span>

<span data-ttu-id="cb8fc-267">Nell'esempio seguente viene illustrato un tipo complesso, **Address**, con le proprietà **EDMSimpleType** **StreetAddress**, **City**, **StateOrProvince**, **Country**e **PostalCode**.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-267">The following example shows a complex type, **Address**, with the **EdmSimpleType** properties **StreetAddress**, **City**, **StateOrProvince**, **Country**, and **PostalCode**.</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

<span data-ttu-id="cb8fc-268">Per definire l' **Indirizzo** del tipo complesso (sopra) come proprietà di un tipo di entità, è necessario dichiarare il tipo di proprietà nella definizione del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-268">To define the complex type **Address** (above) as a property of an entity type, you must declare the property type in the entity type definition.</span></span> <span data-ttu-id="cb8fc-269">Nell'esempio seguente viene illustrata la proprietà **Address** come tipo complesso in un tipo di entità (**Publisher**):</span><span class="sxs-lookup"><span data-stu-id="cb8fc-269">The following example shows the **Address** property as a complex type on an entity type (**Publisher**):</span></span>

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
 

 

## <a name="definingexpression-element-csdl"></a><span data-ttu-id="cb8fc-270">Elemento DefiningExpression (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-270">DefiningExpression Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-271">L'elemento **DefiningExpression** in Conceptual Schema Definition Language (CSDL) contiene un'espressione Entity SQL che definisce una funzione nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-271">The **DefiningExpression** element in conceptual schema definition language (CSDL) contains an Entity SQL expression that defines a function in the conceptual model.</span></span>  

> [!NOTE]
> <span data-ttu-id="cb8fc-272">Ai fini della convalida, un elemento **DefiningExpression** può contenere contenuto arbitrario.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-272">For validation purposes, a **DefiningExpression** element can contain arbitrary content.</span></span> <span data-ttu-id="cb8fc-273">Tuttavia, Entity Framework genererà un'eccezione in fase di esecuzione se un elemento **DefiningExpression** non contiene Entity SQL validi.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-273">However, Entity Framework will throw an exception at runtime if a **DefiningExpression** element does not contain valid Entity SQL.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-274">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-274">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-275">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **DefiningExpression** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-275">Any number of annotation attributes (custom XML attributes) may be applied to the **DefiningExpression** element.</span></span> <span data-ttu-id="cb8fc-276">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-276">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-277">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-277">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="cb8fc-278">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-278">Example</span></span>

<span data-ttu-id="cb8fc-279">Nell'esempio seguente viene utilizzato un elemento **DefiningExpression** per definire una funzione che restituisce il numero di anni da quando un libro è stato pubblicato.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-279">The following example uses a **DefiningExpression** element to define a function that returns the number of years since a book was published.</span></span> <span data-ttu-id="cb8fc-280">Il contenuto dell'elemento **DefiningExpression** è scritto in Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-280">The content of the **DefiningExpression** element is written in Entity SQL.</span></span>

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a><span data-ttu-id="cb8fc-281">Elemento Dependent (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-281">Dependent Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-282">L'elemento **dipendente** in Conceptual Schema Definition Language (CSDL) è un elemento figlio dell'elemento ReferentialConstraint e definisce l'entità finale dipendente di un vincolo referenziale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-282">The **Dependent** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element and defines the dependent end of a referential constraint.</span></span> <span data-ttu-id="cb8fc-283">Un elemento **ReferentialConstraint** definisce la funzionalità che è simile a un vincolo di integrità referenziale in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-283">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="cb8fc-284">Nello stesso modo in cui una colonna (o più colonne) da una tabella di database può fare riferimento alla chiave primaria di un'altra tabella, una proprietà (o più proprietà) di un tipo di entità può fare riferimento alla chiave di entità di un altro tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-284">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="cb8fc-285">Il tipo di entità a cui si fa riferimento viene chiamato *entità finale principale* del vincolo.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-285">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="cb8fc-286">Il tipo di entità che fa riferimento all'entità finale principale viene chiamato entità *finale dipendente* del vincolo.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-286">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="cb8fc-287">Gli elementi **PropertyRef** vengono utilizzati per specificare quali chiavi fanno riferimento all'entità finale principale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-287">**PropertyRef** elements are used to specify which keys reference the principal end.</span></span>

<span data-ttu-id="cb8fc-288">L'elemento **dipendente** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="cb8fc-288">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cb8fc-289">PropertyRef (uno o più elementi)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-289">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="cb8fc-290">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-290">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-291">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-291">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-292">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **dipendente** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-292">The table below describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="cb8fc-293">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="cb8fc-293">Attribute Name</span></span> | <span data-ttu-id="cb8fc-294">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-294">Is Required</span></span> | <span data-ttu-id="cb8fc-295">Value</span><span class="sxs-lookup"><span data-stu-id="cb8fc-295">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="cb8fc-296">**Ruolo**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-296">**Role**</span></span>       | <span data-ttu-id="cb8fc-297">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-297">Yes</span></span>         | <span data-ttu-id="cb8fc-298">Nome del tipo di entità in un'entità finale dipendente dell'associazione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-298">The name of the entity type on the dependent end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="cb8fc-299">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **dipendente** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-299">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="cb8fc-300">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-300">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-301">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-301">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="cb8fc-302">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-302">Example</span></span>

<span data-ttu-id="cb8fc-303">Nell'esempio seguente viene illustrato un elemento **ReferentialConstraint** usato come parte della definizione dell'associazione **PublishedBy** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-303">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="cb8fc-304">La proprietà **publisherID** del tipo di entità **book** costituisce l'entità finale dipendente del vincolo referenziale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-304">The **PublisherId** property of the **Book** entity type makes up the dependent end of the referential constraint.</span></span>

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
 

 

## <a name="documentation-element-csdl"></a><span data-ttu-id="cb8fc-305">Elemento Documentation (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-305">Documentation Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-306">L'elemento **Documentation** in Conceptual Schema Definition Language (CSDL) può essere utilizzato per fornire informazioni su un oggetto definito in un elemento padre.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-306">The **Documentation** element in conceptual schema definition language (CSDL) can be used to provide information about an object that is defined in a parent element.</span></span> <span data-ttu-id="cb8fc-307">In un file con estensione edmx, quando l'elemento **Documentation** è un elemento figlio di un elemento che viene visualizzato come oggetto nell'area di progettazione di Entity Framework Designer (ad esempio un'entità, un'associazione o una proprietà), il contenuto dell'elemento **Documentation** verrà visualizzato nel Finestra delle **Proprietà** di Visual Studio per l'oggetto.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-307">In an .edmx file, when the **Documentation** element is a child of an element that appears as an object on the design surface of the EF Designer  (such as an entity, association, or property), the contents of the **Documentation** element will appear in the Visual Studio **Properties** window for the object.</span></span>

<span data-ttu-id="cb8fc-308">L'elemento **Documentation** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="cb8fc-308">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cb8fc-309">**Riepilogo**: Breve descrizione dell'elemento padre.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-309">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="cb8fc-310">Zero o un elemento.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-310">(zero or one element)</span></span>
-   <span data-ttu-id="cb8fc-311">**LongDescription**: Descrizione completa dell'elemento padre.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-311">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="cb8fc-312">Zero o un elemento.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-312">(zero or one element)</span></span>
-   <span data-ttu-id="cb8fc-313">Elementi di annotazione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-313">Annotation elements.</span></span> <span data-ttu-id="cb8fc-314">Zero o più elementi.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-314">(zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-315">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-315">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-316">All'elemento **Documentation** è possibile applicare qualsiasi numero di attributi di annotazione (attributi XML personalizzati).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-316">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="cb8fc-317">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-317">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-318">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-318">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="cb8fc-319">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-319">Example</span></span>

<span data-ttu-id="cb8fc-320">Nell'esempio seguente viene illustrato l'elemento **Documentation** come elemento figlio di un elemento EntityType.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-320">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span> <span data-ttu-id="cb8fc-321">Se il frammento di codice seguente si trova nel contenuto CSDL di un file con estensione edmx, il contenuto degli elementi **Summary** e **LongDescription** verrà visualizzato nella finestra **proprietà** di Visual Studio quando si fa clic sul tipo di entità `Customer`.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-321">If the snippet below were in the CSDL content of an .edmx file, the contents of the **Summary** and **LongDescription** elements would appear in the Visual Studio **Properties** window when you click on the `Customer` entity type.</span></span>

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
 

 

## <a name="end-element-csdl"></a><span data-ttu-id="cb8fc-322">Elemento End (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-322">End Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-323">L'elemento **end** in Conceptual Schema Definition Language (CSDL) può essere un elemento figlio dell'elemento Association o dell'elemento associationname.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-323">The **End** element in conceptual schema definition language (CSDL) can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="cb8fc-324">In ogni caso, il ruolo dell'elemento **end** è diverso e gli attributi applicabili sono diversi.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-324">In each case, the role of the **End** element is different and the applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="cb8fc-325">Elemento End come figlio dell'elemento Association</span><span class="sxs-lookup"><span data-stu-id="cb8fc-325">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="cb8fc-326">Un elemento **end** (come figlio dell'elemento **Association** ) identifica il tipo di entità in un'entità finale di un'associazione e il numero di istanze del tipo di entità che possono esistere a tale estremità di un'associazione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-326">An **End** element (as a child of the **Association** element) identifies the entity type on one end of an association and the number of entity type instances that can exist at that end of an association.</span></span> <span data-ttu-id="cb8fc-327">Le entità finali dell'associazione sono definite come parte di un'associazione; un'associazione deve disporre esattamente di due entità finali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-327">Association ends are defined as part of an association; an association must have exactly two association ends.</span></span> <span data-ttu-id="cb8fc-328">Le istanze del tipo di entità in corrispondenza di un'entità finale di un'associazione sono accessibili attraverso proprietà di navigazione o chiavi esterne se sono esposte in un tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-328">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys if they are exposed on an entity type.</span></span>  

<span data-ttu-id="cb8fc-329">Un elemento **end** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="cb8fc-329">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cb8fc-330">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-330">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="cb8fc-331">OnDelete (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-331">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="cb8fc-332">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-332">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-333">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-333">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-334">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **finale** quando è figlio di un elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-334">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="cb8fc-335">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="cb8fc-335">Attribute Name</span></span>   | <span data-ttu-id="cb8fc-336">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-336">Is Required</span></span> | <span data-ttu-id="cb8fc-337">Value</span><span class="sxs-lookup"><span data-stu-id="cb8fc-337">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cb8fc-338">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-338">**Type**</span></span>         | <span data-ttu-id="cb8fc-339">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-339">Yes</span></span>         | <span data-ttu-id="cb8fc-340">Nome del tipo di entità in una entità finale dell'associazione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-340">The name of the entity type at one end of the association.</span></span>                                                                                                                                                                                                                                                                                                                                                         |
| <span data-ttu-id="cb8fc-341">**Ruolo**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-341">**Role**</span></span>         | <span data-ttu-id="cb8fc-342">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-342">No</span></span>          | <span data-ttu-id="cb8fc-343">Nome per l'entità finale dell'associazione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-343">A name for the association end.</span></span> <span data-ttu-id="cb8fc-344">Se non è fornito alcun nome, verrà utilizzato il nome del tipo di entità nell'entità finale dell'associazione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-344">If no name is provided, the name of the entity type on the association end will be used.</span></span>                                                                                                                                                                                                                                                                                           |
| <span data-ttu-id="cb8fc-345">**Molteplicità**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-345">**Multiplicity**</span></span> | <span data-ttu-id="cb8fc-346">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-346">Yes</span></span>         | <span data-ttu-id="cb8fc-347">**1**, **0.. 1**o **\*** a seconda del numero di istanze del tipo di entità che possono trovarsi alla fine dell'associazione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-347">**1**, **0..1**, or **\*** depending on the number of entity type instances that can be at the end of the association.</span></span> <br/> <span data-ttu-id="cb8fc-348">**1** indica che nell'entità finale dell'associazione esiste esattamente un'istanza del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-348">**1** indicates that exactly one entity type instance exists at the association end.</span></span> <br/> <span data-ttu-id="cb8fc-349">**0.. 1** indica che nell'entità finale dell'associazione sono presenti zero o un'istanza del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-349">**0..1** indicates that zero or one entity type instances exist at the association end.</span></span> <br/> <span data-ttu-id="cb8fc-350">**\*** indica che esistono zero, una o più istanze del tipo di entità nell'entità finale dell'associazione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-350">**\*** indicates that zero, one, or more entity type instances exist at the association end.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="cb8fc-351">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **finale** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-351">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="cb8fc-352">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-352">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-353">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-353">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="cb8fc-354">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-354">Example</span></span>

<span data-ttu-id="cb8fc-355">Nell'esempio seguente viene illustrato un elemento **Association** che definisce l'associazione **CustomerOrders** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-355">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="cb8fc-356">I valori di **molteplicità** per ogni **estremità** dell'associazione indicano che molti **ordini** possono essere associati a un **cliente**, ma solo un **cliente** può essere associato a un **ordine**.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-356">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="cb8fc-357">Inoltre, l'elemento **OnDelete** indica che tutti **gli ordini** relativi a un determinato **cliente** e che sono stati caricati in ObjectContext verranno eliminati se il **cliente** viene eliminato.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-357">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and that have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="cb8fc-358">Elemento End come figlio dell'elemento AssociationSet</span><span class="sxs-lookup"><span data-stu-id="cb8fc-358">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="cb8fc-359">L'elemento **end** specifica un'estremità di un set di associazioni.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-359">The **End** element specifies one end of an association set.</span></span> <span data-ttu-id="cb8fc-360">L'elemento **associationname** deve contenere due elementi **end** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-360">The **AssociationSet** element must contain two **End** elements.</span></span> <span data-ttu-id="cb8fc-361">Le informazioni contenute in un elemento **end** vengono utilizzate per eseguire il mapping di un set di associazioni a un'origine dati.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-361">The information contained in an **End** element is used in mapping an association set to a data source.</span></span>

<span data-ttu-id="cb8fc-362">Un elemento **end** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="cb8fc-362">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cb8fc-363">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-363">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="cb8fc-364">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-364">Annotation elements (zero or more elements)</span></span>

> [!NOTE]
> <span data-ttu-id="cb8fc-365">Gli elementi Annotation devono apparire dopo tutti gli altri elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-365">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="cb8fc-366">Gli elementi Annotation sono consentiti solo in CSDL V2 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-366">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-367">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-367">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-368">Nella tabella seguente vengono descritti gli attributi che possono essere applicati all'elemento **finale** quando è figlio di un elemento di **associazione** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-368">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="cb8fc-369">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="cb8fc-369">Attribute Name</span></span> | <span data-ttu-id="cb8fc-370">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-370">Is Required</span></span> | <span data-ttu-id="cb8fc-371">Value</span><span class="sxs-lookup"><span data-stu-id="cb8fc-371">Value</span></span>                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cb8fc-372">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-372">**EntitySet**</span></span>  | <span data-ttu-id="cb8fc-373">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-373">Yes</span></span>         | <span data-ttu-id="cb8fc-374">Nome dell'elemento **EntitySet** che definisce un'entità finale dell'elemento di **associazione** padre.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-374">The name of the **EntitySet** element that defines one end of the parent **AssociationSet** element.</span></span> <span data-ttu-id="cb8fc-375">L'elemento **EntitySet** deve essere definito nello stesso contenitore di entità dell'elemento di **associazione** padre.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-375">The **EntitySet** element must be defined in the same entity container as the parent **AssociationSet** element.</span></span> |
| <span data-ttu-id="cb8fc-376">**Ruolo**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-376">**Role**</span></span>       | <span data-ttu-id="cb8fc-377">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-377">No</span></span>          | <span data-ttu-id="cb8fc-378">Nome dell'entità finale del set di associazioni.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-378">The name of the association set end.</span></span> <span data-ttu-id="cb8fc-379">Se non si utilizza l'attributo **Role** , il nome dell'entità finale del set di associazioni sarà il nome del set di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-379">If the **Role** attribute is not used, the name of the association set end will be the name of the entity set.</span></span>                                                                   |

 

> [!NOTE]
> <span data-ttu-id="cb8fc-380">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **finale** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-380">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="cb8fc-381">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-381">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-382">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-382">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="cb8fc-383">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-383">Example</span></span>

<span data-ttu-id="cb8fc-384">Nell'esempio seguente viene illustrato un elemento **EntityContainer** con due elementi di **associazione** , ognuno con due elementi **end** :</span><span class="sxs-lookup"><span data-stu-id="cb8fc-384">The following example shows an **EntityContainer** element with two **AssociationSet** elements, each with two **End** elements:</span></span>

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
 

 

## <a name="entitycontainer-element-csdl"></a><span data-ttu-id="cb8fc-385">Elemento EntityContainer (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-385">EntityContainer Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-386">L'elemento **EntityContainer** in Conceptual Schema Definition Language (CSDL) è un contenitore logico per set di entità, set di associazioni e importazioni di funzioni.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-386">The **EntityContainer** element in conceptual schema definition language (CSDL) is a logical container for entity sets, association sets, and function imports.</span></span> <span data-ttu-id="cb8fc-387">Un contenitore di entità del modello concettuale esegue il mapping a un contenitore di entità del modello di archiviazione tramite l'elemento EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-387">A conceptual model entity container maps to a storage model entity container through the EntityContainerMapping element.</span></span> <span data-ttu-id="cb8fc-388">Un contenitore di entità del modello di archiviazione descrive la struttura del database: i set di entità descrivono le tabelle, i set di associazioni descrivono i vincoli delle chiavi esterne e le importazioni di funzioni descrivono le stored procedure in un database.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-388">A storage model entity container describes the structure of the database: entity sets describe tables, association sets describe foreign key constraints, and function imports describe stored procedures in a database.</span></span>

<span data-ttu-id="cb8fc-389">Un elemento **EntityContainer** può avere uno o più elementi di documentazione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-389">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="cb8fc-390">Se è presente un elemento di **documentazione** , deve precedere tutti gli elementi **EntitySet**, **associationname**e **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-390">If a **Documentation** element is present, it must precede all **EntitySet**, **AssociationSet**, and **FunctionImport** elements.</span></span>

<span data-ttu-id="cb8fc-391">Un elemento **EntityContainer** può avere zero o più degli elementi figlio seguenti (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="cb8fc-391">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cb8fc-392">EntitySet</span><span class="sxs-lookup"><span data-stu-id="cb8fc-392">EntitySet</span></span>
-   <span data-ttu-id="cb8fc-393">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="cb8fc-393">AssociationSet</span></span>
-   <span data-ttu-id="cb8fc-394">FunctionImport</span><span class="sxs-lookup"><span data-stu-id="cb8fc-394">FunctionImport</span></span>
-   <span data-ttu-id="cb8fc-395">Elementi Annotation</span><span class="sxs-lookup"><span data-stu-id="cb8fc-395">Annotation elements</span></span>

<span data-ttu-id="cb8fc-396">È possibile estendere un elemento **EntityContainer** per includere il contenuto di un altro oggetto **EntityContainer** che si trova all'interno dello stesso spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-396">You can extend an **EntityContainer** element to include the contents of another **EntityContainer** that is within the same namespace.</span></span> <span data-ttu-id="cb8fc-397">Per includere il contenuto di un altro **EntityContainer**, nell'elemento **EntityContainer** di riferimento, impostare il valore dell'attributo **extends** sul nome dell'elemento **EntityContainer** che si desidera includere.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-397">To include the contents of another **EntityContainer**, in the referencing **EntityContainer** element, set the value of the **Extends** attribute to the name of the **EntityContainer** element that you want to include.</span></span> <span data-ttu-id="cb8fc-398">Tutti gli elementi figlio dell'elemento **EntityContainer** incluso verranno considerati come elementi figlio dell'elemento **EntityContainer** di riferimento.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-398">All child elements of the included **EntityContainer** element will be treated as child elements of the referencing **EntityContainer** element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-399">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-399">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-400">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **using** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-400">The table below describes the attributes that can be applied to the **Using** element.</span></span>

| <span data-ttu-id="cb8fc-401">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="cb8fc-401">Attribute Name</span></span> | <span data-ttu-id="cb8fc-402">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-402">Is Required</span></span> | <span data-ttu-id="cb8fc-403">Value</span><span class="sxs-lookup"><span data-stu-id="cb8fc-403">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="cb8fc-404">**Name**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-404">**Name**</span></span>       | <span data-ttu-id="cb8fc-405">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-405">Yes</span></span>         | <span data-ttu-id="cb8fc-406">Nome del contenitore di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-406">The name of the entity container.</span></span>                               |
| <span data-ttu-id="cb8fc-407">**Estende**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-407">**Extends**</span></span>    | <span data-ttu-id="cb8fc-408">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-408">No</span></span>          | <span data-ttu-id="cb8fc-409">Nome di un altro contenitore di entità all'interno dello stesso spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-409">The name of another entity container within the same namespace.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="cb8fc-410">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **EntityContainer** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-410">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="cb8fc-411">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-411">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-412">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-412">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="cb8fc-413">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-413">Example</span></span>

<span data-ttu-id="cb8fc-414">Nell'esempio seguente viene illustrato un elemento **EntityContainer** che definisce tre set di entità e due set di associazioni.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-414">The following example shows an **EntityContainer** element that defines three entity sets and two association sets.</span></span>

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
 

 

## <a name="entityset-element-csdl"></a><span data-ttu-id="cb8fc-415">Elemento EntitySet (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-415">EntitySet Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-416">L'elemento **EntitySet** in Conceptual Schema Definition Language è un contenitore logico per le istanze di un tipo di entità e le istanze di qualsiasi tipo derivato da tale tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-416">The **EntitySet** element in conceptual schema definition language is a logical container for instances of an entity type and instances of any type that is derived from that entity type.</span></span> <span data-ttu-id="cb8fc-417">La relazione tra un tipo di entità e un set di entità è analoga alla relazione tra una riga e una tabella in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-417">The relationship between an entity type and an entity set is analogous to the relationship between a row and a table in a relational database.</span></span> <span data-ttu-id="cb8fc-418">Analogamente a una riga, un tipo di entità definisce un set di dati correlati e, analogamente a una tabella, un set di entità contiene istanze di quella definizione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-418">Like a row, an entity type defines a set of related data, and, like a table, an entity set contains instances of that definition.</span></span> <span data-ttu-id="cb8fc-419">Un set di entità fornisce un construct per il raggruppamento di istanze del tipo di entità in modo che se ne possa eseguire il mapping alle strutture dei dati correlati in un'origine dati.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-419">An entity set provides a construct for grouping entity type instances so that they can be mapped to related data structures in a data source.</span></span>  

<span data-ttu-id="cb8fc-420">È possibile definire più set di entità per un particolare tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-420">More than one entity set for a particular entity type may be defined.</span></span>

> [!NOTE]
> <span data-ttu-id="cb8fc-421">EF designer non supporta i modelli concettuali che contengono Multiple Entity Sets per Type.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-421">The EF Designer does not support conceptual models that contain multiple entity sets per type.</span></span>

 

<span data-ttu-id="cb8fc-422">L'elemento **EntitySet** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="cb8fc-422">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cb8fc-423">Elemento Documentation (zero o uno degli elementi consentiti)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-423">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="cb8fc-424">Elementi Annotation (zero o più elementi consentiti)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-424">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-425">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-425">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-426">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-426">The table below describes the attributes that can be applied to the **EntitySet** element.</span></span>

| <span data-ttu-id="cb8fc-427">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="cb8fc-427">Attribute Name</span></span> | <span data-ttu-id="cb8fc-428">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-428">Is Required</span></span> | <span data-ttu-id="cb8fc-429">Value</span><span class="sxs-lookup"><span data-stu-id="cb8fc-429">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="cb8fc-430">**Name**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-430">**Name**</span></span>       | <span data-ttu-id="cb8fc-431">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-431">Yes</span></span>         | <span data-ttu-id="cb8fc-432">Nome del set di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-432">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="cb8fc-433">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-433">**EntityType**</span></span> | <span data-ttu-id="cb8fc-434">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-434">Yes</span></span>         | <span data-ttu-id="cb8fc-435">Nome completo del tipo di entità per il quale il set di entità contiene delle istanze.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-435">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="cb8fc-436">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-436">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="cb8fc-437">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-437">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-438">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-438">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="cb8fc-439">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-439">Example</span></span>

<span data-ttu-id="cb8fc-440">Nell'esempio seguente viene illustrato un elemento **EntityContainer** con tre elementi **EntitySet** :</span><span class="sxs-lookup"><span data-stu-id="cb8fc-440">The following example shows an **EntityContainer** element with three **EntitySet** elements:</span></span>

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
 

<span data-ttu-id="cb8fc-441">È possibile definire più set di entità per tipo (MEST).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-441">It is possible to define multiple entity sets per type (MEST).</span></span> <span data-ttu-id="cb8fc-442">Nell'esempio seguente viene definito un contenitore di entità con due set di entità per il tipo di entità **book** :</span><span class="sxs-lookup"><span data-stu-id="cb8fc-442">The following example defines an entity container with two entity sets for the **Book** entity type:</span></span>

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
 

 

## <a name="entitytype-element-csdl"></a><span data-ttu-id="cb8fc-443">Elemento EntityType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-443">EntityType Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-444">L'elemento **EntityType** rappresenta la struttura di un concetto di primo livello, ad esempio un cliente o un ordine, in un modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-444">The **EntityType** element represents the structure of a top-level concept, such as a customer or order, in a conceptual model.</span></span> <span data-ttu-id="cb8fc-445">Un tipo di entità è un modello per istanze di tipi di entità in un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-445">An entity type is a template for instances of entity types in an application.</span></span> <span data-ttu-id="cb8fc-446">Ogni modello contiene le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="cb8fc-446">Each template contains the following information:</span></span>

-   <span data-ttu-id="cb8fc-447">Un nome univoco.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-447">A unique name.</span></span> <span data-ttu-id="cb8fc-448">Obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-448">(Required.)</span></span>
-   <span data-ttu-id="cb8fc-449">Una chiave di entità che è definita da una o più proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-449">An entity key that is defined by one or more properties.</span></span> <span data-ttu-id="cb8fc-450">Obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-450">(Required.)</span></span>
-   <span data-ttu-id="cb8fc-451">Proprietà per contenere dati.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-451">Properties for containing data.</span></span> <span data-ttu-id="cb8fc-452">(Facoltative)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-452">(Optional.)</span></span>
-   <span data-ttu-id="cb8fc-453">Proprietà di navigazione che consentono di navigare da un'entità finale di un'associazione all'altra.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-453">Navigation properties that allow for navigation from one end of an association to the other end.</span></span> <span data-ttu-id="cb8fc-454">(Facoltative)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-454">(Optional.)</span></span>

<span data-ttu-id="cb8fc-455">In un'applicazione, un'istanza di un tipo di entità rappresenta un oggetto specifico, quale ad esempio un cliente o un ordine specifico.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-455">In an application, an instance of an entity type represents a specific object (such as a specific customer or order).</span></span> <span data-ttu-id="cb8fc-456">Ogni istanza di un tipo di entità deve avere una chiave di entità univoca all'interno di un set di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-456">Each instance of an entity type must have a unique entity key within an entity set.</span></span>

<span data-ttu-id="cb8fc-457">Due istanze di tipi di entità sono considerate uguali solo se sono dello stesso tipo e se i valori delle rispettive chiavi di entità sono uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-457">Two entity type instances are considered equal only if they are of the same type and the values of their entity keys are the same.</span></span>

<span data-ttu-id="cb8fc-458">Un elemento **EntityType** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="cb8fc-458">An **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cb8fc-459">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-459">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="cb8fc-460">Key (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-460">Key (zero or one element)</span></span>
-   <span data-ttu-id="cb8fc-461">Property (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-461">Property (zero or more elements)</span></span>
-   <span data-ttu-id="cb8fc-462">NavigationProperty (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-462">NavigationProperty (zero or more elements)</span></span>
-   <span data-ttu-id="cb8fc-463">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-463">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-464">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-464">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-465">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-465">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="cb8fc-466">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="cb8fc-466">Attribute Name</span></span>                                                                                                                                  | <span data-ttu-id="cb8fc-467">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-467">Is Required</span></span> | <span data-ttu-id="cb8fc-468">Value</span><span class="sxs-lookup"><span data-stu-id="cb8fc-468">Value</span></span>                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cb8fc-469">**Name**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-469">**Name**</span></span>                                                                                                                                        | <span data-ttu-id="cb8fc-470">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-470">Yes</span></span>         | <span data-ttu-id="cb8fc-471">Nome del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-471">The name of the entity type.</span></span>                                                                     |
| <span data-ttu-id="cb8fc-472">**BaseType**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-472">**BaseType**</span></span>                                                                                                                                    | <span data-ttu-id="cb8fc-473">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-473">No</span></span>          | <span data-ttu-id="cb8fc-474">Nome di un altro tipo di entità che è il tipo di base del tipo di entità definito.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-474">The name of another entity type that is the base type of the entity type that is being defined.</span></span>  |
| <span data-ttu-id="cb8fc-475">**Astratta**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-475">**Abstract**</span></span>                                                                                                                                    | <span data-ttu-id="cb8fc-476">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-476">No</span></span>          | <span data-ttu-id="cb8fc-477">**True** o **false**, a seconda che il tipo di entità sia un tipo astratto.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-477">**True** or **False**, depending on whether the entity type is an abstract type.</span></span>                 |
| <span data-ttu-id="cb8fc-478">**OpenType**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-478">**OpenType**</span></span>                                                                                                                                    | <span data-ttu-id="cb8fc-479">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-479">No</span></span>          | <span data-ttu-id="cb8fc-480">**True** o **false** a seconda che il tipo di entità sia un tipo di entità aperto.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-480">**True** or **False** depending on whether the entity type is an open entity type.</span></span> <br/> [!NOTE] |
| <span data-ttu-id="cb8fc-481">> L'attributo **OpenType** è applicabile solo ai tipi di entità definiti nei modelli concettuali utilizzati con ADO.NET Data Services.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-481">> The **OpenType** attribute is only applicable to entity types that are defined in conceptual models that are used with ADO.NET Data Services.</span></span> |             |                                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="cb8fc-482">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-482">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="cb8fc-483">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-483">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-484">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-484">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="cb8fc-485">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-485">Example</span></span>

<span data-ttu-id="cb8fc-486">Nell'esempio seguente viene illustrato un elemento **EntityType** con tre elementi **Property** e due elementi **NavigationProperty** :</span><span class="sxs-lookup"><span data-stu-id="cb8fc-486">The following example shows an **EntityType** element with three **Property** elements and two **NavigationProperty** elements:</span></span>

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
 

 

## <a name="enumtype-element-csdl"></a><span data-ttu-id="cb8fc-487">Elemento EnumType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-487">EnumType Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-488">L'elemento **enumType** rappresenta un tipo enumerato.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-488">The **EnumType** element represents an enumerated type.</span></span>

<span data-ttu-id="cb8fc-489">Un elemento **enumType** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="cb8fc-489">An **EnumType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cb8fc-490">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-490">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="cb8fc-491">Member (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-491">Member (zero or more elements)</span></span>
-   <span data-ttu-id="cb8fc-492">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-492">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-493">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-493">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-494">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **enumType** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-494">The table below describes the attributes that can be applied to the **EnumType** element.</span></span>

| <span data-ttu-id="cb8fc-495">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="cb8fc-495">Attribute Name</span></span>     | <span data-ttu-id="cb8fc-496">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-496">Is Required</span></span> | <span data-ttu-id="cb8fc-497">Value</span><span class="sxs-lookup"><span data-stu-id="cb8fc-497">Value</span></span>                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cb8fc-498">**Name**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-498">**Name**</span></span>           | <span data-ttu-id="cb8fc-499">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-499">Yes</span></span>         | <span data-ttu-id="cb8fc-500">Nome del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-500">The name of the entity type.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="cb8fc-501">**IsFlags**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-501">**IsFlags**</span></span>        | <span data-ttu-id="cb8fc-502">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-502">No</span></span>          | <span data-ttu-id="cb8fc-503">**True** o **false**, a seconda che il tipo enum possa essere utilizzato come set di flag.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-503">**True** or **False**, depending on whether the enum type can be used as a set of flags.</span></span> <span data-ttu-id="cb8fc-504">Il valore predefinito è **false.**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-504">The default value is **False.**.</span></span>                                                                     |
| <span data-ttu-id="cb8fc-505">**UnderlyingType**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-505">**UnderlyingType**</span></span> | <span data-ttu-id="cb8fc-506">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-506">No</span></span>          | <span data-ttu-id="cb8fc-507">**EDM. byte**, **EDM. Int16**, **EDM. Int32**, **EDM. Int64** o **EDM. SByte** che definisce l'intervallo di valori del tipo.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** or **Edm.SByte** defining the range of values of the type.</span></span> <span data-ttu-id="cb8fc-508">  Il tipo sottostante predefinito degli elementi di enumerazione è **EDM. Int32.** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-508">  The default underlying type of enumeration elements is **Edm.Int32.**.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="cb8fc-509">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **enumType** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-509">Any number of annotation attributes (custom XML attributes) may be applied to the **EnumType** element.</span></span> <span data-ttu-id="cb8fc-510">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-510">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-511">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-511">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="cb8fc-512">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-512">Example</span></span>

<span data-ttu-id="cb8fc-513">Nell'esempio seguente viene illustrato un elemento **enumType** con tre elementi **member** :</span><span class="sxs-lookup"><span data-stu-id="cb8fc-513">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a><span data-ttu-id="cb8fc-514">Elemento Function (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-514">Function Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-515">L'elemento **Function** in Conceptual Schema Definition Language (CSDL) viene utilizzato per definire o dichiarare funzioni nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-515">The **Function** element in conceptual schema definition language (CSDL) is used to define or declare functions in the conceptual model.</span></span> <span data-ttu-id="cb8fc-516">Una funzione viene definita utilizzando un elemento DefiningExpression.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-516">A function is defined by using a DefiningExpression element.</span></span>  

<span data-ttu-id="cb8fc-517">Un elemento **Function** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="cb8fc-517">A **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cb8fc-518">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-518">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="cb8fc-519">Parameter (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-519">Parameter (zero or more elements)</span></span>
-   <span data-ttu-id="cb8fc-520">DefiningExpression (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-520">DefiningExpression (zero or one element)</span></span>
-   <span data-ttu-id="cb8fc-521">ReturnType (funzione) (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-521">ReturnType (Function) (zero or one element)</span></span>
-   <span data-ttu-id="cb8fc-522">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-522">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="cb8fc-523">Un tipo restituito per una funzione deve essere specificato con l'elemento **returnType** (Function) o l'attributo **returnType** (vedere di seguito), ma non entrambi.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-523">A return type for a function must be specified with either the **ReturnType** (Function) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="cb8fc-524">I tipi restituiti possibili sono il edmSimpleType, il tipo di entità, il tipo complesso, il tipo di riga o il tipo di riferimento o una raccolta di uno di questi tipi.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-524">The possible return types are any EdmSimpleType, entity type, complex type, row type, or ref type (or a collection of one of these types).</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-525">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-525">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-526">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **Function** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-526">The table below describes the attributes that can be applied to the **Function** element.</span></span>

| <span data-ttu-id="cb8fc-527">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="cb8fc-527">Attribute Name</span></span> | <span data-ttu-id="cb8fc-528">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-528">Is Required</span></span> | <span data-ttu-id="cb8fc-529">Value</span><span class="sxs-lookup"><span data-stu-id="cb8fc-529">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="cb8fc-530">**Name**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-530">**Name**</span></span>       | <span data-ttu-id="cb8fc-531">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-531">Yes</span></span>         | <span data-ttu-id="cb8fc-532">Nome della funzione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-532">The name of the function.</span></span>          |
| <span data-ttu-id="cb8fc-533">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-533">**ReturnType**</span></span> | <span data-ttu-id="cb8fc-534">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-534">No</span></span>          | <span data-ttu-id="cb8fc-535">Tipo restituito dalla funzione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-535">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="cb8fc-536">All'elemento **Function** è possibile applicare qualsiasi numero di attributi di annotazione (attributi XML personalizzati).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-536">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="cb8fc-537">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-537">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-538">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-538">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="cb8fc-539">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-539">Example</span></span>

<span data-ttu-id="cb8fc-540">Nell'esempio seguente viene utilizzato un elemento **Function** per definire una funzione che restituisce il numero di anni dall'assunzione di un insegnante.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-540">The following example uses a **Function** element to define a function that returns the number of years since an instructor was hired.</span></span>

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a><span data-ttu-id="cb8fc-541">Elemento FunctionImport (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-541">FunctionImport Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-542">L'elemento **FunctionImport** in Conceptual Schema Definition Language (CSDL) rappresenta una funzione definita nell'origine dati, ma disponibile per gli oggetti tramite il modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-542">The **FunctionImport** element in conceptual schema definition language (CSDL) represents a function that is defined in the data source but available to objects through the conceptual model.</span></span> <span data-ttu-id="cb8fc-543">È ad esempio possibile utilizzare un elemento Function nel modello di archiviazione per rappresentare un stored procedure in un database.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-543">For example, a Function element in the storage model can be used to represent a stored procedure in a database.</span></span> <span data-ttu-id="cb8fc-544">Un elemento **FunctionImport** nel modello concettuale rappresenta la funzione corrispondente in un'applicazione Entity Framework e viene mappato alla funzione del modello di archiviazione utilizzando l'elemento FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-544">A **FunctionImport** element in the conceptual model represents the corresponding function in an Entity Framework application and is mapped to the storage model function by using the FunctionImportMapping element.</span></span> <span data-ttu-id="cb8fc-545">Quando la funzione viene chiamata nell'applicazione, la stored procedure corrispondente viene eseguita nel database.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-545">When the function is called in the application, the corresponding stored procedure is executed in the database.</span></span>

<span data-ttu-id="cb8fc-546">L'elemento **FunctionImport** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="cb8fc-546">The **FunctionImport** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cb8fc-547">Documentazione (zero o un elemento consentito)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-547">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="cb8fc-548">Parameter (zero o più elementi consentiti)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-548">Parameter (zero or more elements allowed)</span></span>
-   <span data-ttu-id="cb8fc-549">Elementi Annotation (zero o più elementi consentiti)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-549">Annotation elements (zero or more elements allowed)</span></span>
-   <span data-ttu-id="cb8fc-550">ReturnType (FunctionImport) (zero o più elementi consentiti)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-550">ReturnType (FunctionImport) (zero or more elements allowed)</span></span>

<span data-ttu-id="cb8fc-551">È necessario definire un elemento **Parameter** per ogni parametro accettato dalla funzione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-551">One **Parameter** element should be defined for each parameter that the function accepts.</span></span>

<span data-ttu-id="cb8fc-552">Un tipo restituito per una funzione deve essere specificato con l'elemento **returnType** (FunctionImport) o l'attributo **returnType** (vedere di seguito), ma non entrambi.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-552">A return type for a function must be specified with either the **ReturnType** (FunctionImport) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="cb8fc-553">Il valore del tipo restituito deve essere una raccolta di EdmSimpleType, EntityType o ComplexType.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-553">The return type value must be a  collection of EdmSimpleType, EntityType, or ComplexType.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-554">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-554">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-555">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-555">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="cb8fc-556">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="cb8fc-556">Attribute Name</span></span>   | <span data-ttu-id="cb8fc-557">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-557">Is Required</span></span> | <span data-ttu-id="cb8fc-558">Value</span><span class="sxs-lookup"><span data-stu-id="cb8fc-558">Value</span></span>                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cb8fc-559">**Name**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-559">**Name**</span></span>         | <span data-ttu-id="cb8fc-560">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-560">Yes</span></span>         | <span data-ttu-id="cb8fc-561">Nome della funzione importata.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-561">The name of the imported function.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="cb8fc-562">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-562">**ReturnType**</span></span>   | <span data-ttu-id="cb8fc-563">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-563">No</span></span>          | <span data-ttu-id="cb8fc-564">Tipo restituito dalla funzione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-564">The type that the function returns.</span></span> <span data-ttu-id="cb8fc-565">Non utilizzare questo attributo se la funzione non restituisce un valore.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-565">Do not use this attribute if the function does not return a value.</span></span> <span data-ttu-id="cb8fc-566">In caso contrario, il valore deve essere una raccolta di ComplexType, EntityType o EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-566">Otherwise, the value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>        |
| <span data-ttu-id="cb8fc-567">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-567">**EntitySet**</span></span>    | <span data-ttu-id="cb8fc-568">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-568">No</span></span>          | <span data-ttu-id="cb8fc-569">Se la funzione restituisce una raccolta di tipi di entità, il valore di **EntitySet** deve essere il set di entità a cui appartiene la raccolta.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-569">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="cb8fc-570">In caso contrario, non è necessario utilizzare l'attributo **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-570">Otherwise, the **EntitySet** attribute must not be used.</span></span> |
| <span data-ttu-id="cb8fc-571">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-571">**IsComposable**</span></span> | <span data-ttu-id="cb8fc-572">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-572">No</span></span>          | <span data-ttu-id="cb8fc-573">Se il valore è impostato su true, la funzione è componibile (funzione con valori di tabella) e può essere utilizzata in una query LINQ.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-573">If the value is set to true, the function is composable (Table-valued Function) and can be used in a LINQ query.</span></span><span data-ttu-id="cb8fc-574">  Il valore predefinito è **false**.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-574">  The default is **false**.</span></span>                                                           |

 

> [!NOTE]
> <span data-ttu-id="cb8fc-575">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-575">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="cb8fc-576">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-576">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-577">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-577">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="cb8fc-578">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-578">Example</span></span>

<span data-ttu-id="cb8fc-579">Nell'esempio seguente viene illustrato un elemento **FunctionImport** che accetta un parametro e restituisce una raccolta di tipi di entità:</span><span class="sxs-lookup"><span data-stu-id="cb8fc-579">The following example shows a **FunctionImport** element that accepts one parameter and returns a collection of entity types:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a><span data-ttu-id="cb8fc-580">Elemento Key (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-580">Key Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-581">L'elemento **Key** è un elemento figlio dell'elemento EntityType e definisce una *chiave di entità* , ovvero una proprietà o un set di proprietà di un tipo di entità che determinano l'identità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-581">The **Key** element is a child element of the EntityType element and defines an *entity key* (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="cb8fc-582">Le proprietà che costituiscono una chiave di entità vengono scelte in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-582">The properties that make up an entity key are chosen at design time.</span></span> <span data-ttu-id="cb8fc-583">I valori delle proprietà della chiave di entità devono identificare in modo univoco in fase di esecuzione un'istanza del tipo di entità all'interno di un set di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-583">The values of entity key properties must uniquely identify an entity type instance within an entity set at run time.</span></span> <span data-ttu-id="cb8fc-584">Le proprietà che costituiscono una chiave di entità devono essere scelte per garantire univocità delle istanze in un set di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-584">The properties that make up an entity key should be chosen to guarantee uniqueness of instances in an entity set.</span></span> <span data-ttu-id="cb8fc-585">L'elemento **Key** definisce una chiave di entità facendo riferimento a una o più proprietà di un tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-585">The **Key** element defines an entity key by referencing one or more of the properties of an entity type.</span></span>

<span data-ttu-id="cb8fc-586">L'elemento **chiave** può avere gli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="cb8fc-586">The **Key** element can have the following child elements:</span></span>

-   <span data-ttu-id="cb8fc-587">PropertyRef (uno o più elementi)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-587">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="cb8fc-588">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-588">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-589">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-589">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-590">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **chiave** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-590">Any number of annotation attributes (custom XML attributes) may be applied to the **Key** element.</span></span> <span data-ttu-id="cb8fc-591">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-591">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-592">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-592">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="cb8fc-593">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-593">Example</span></span>

<span data-ttu-id="cb8fc-594">Nell'esempio seguente viene definito un tipo di entità denominato **book**.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-594">The example below defines an entity type named **Book**.</span></span> <span data-ttu-id="cb8fc-595">La chiave di entità viene definita facendo riferimento alla proprietà **ISBN** del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-595">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="cb8fc-596">La proprietà **ISBN** è una scelta ottimale per la chiave di entità perché un numero di libro standard internazionale (ISBN) identifica in modo univoco un libro.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-596">The **ISBN** property is a good choice for the entity key because an International Standard Book Number (ISBN) uniquely identifies a book.</span></span>

<span data-ttu-id="cb8fc-597">Nell'esempio seguente viene illustrato un tipo di entità (**autore**) che dispone di una chiave di entità costituita da due proprietà, **Name** e **Address**.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-597">The following example shows an entity type (**Author**) that has an entity key that consists of two properties, **Name** and **Address**.</span></span>

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
 

<span data-ttu-id="cb8fc-598">L'uso del **nome** e dell' **Indirizzo** per la chiave di entità è una scelta ragionevole, perché due autori con lo stesso nome non possono vivere allo stesso indirizzo.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-598">Using **Name** and **Address** for the entity key is a reasonable choice, because two authors of the same name are unlikely to live at the same address.</span></span> <span data-ttu-id="cb8fc-599">Questa scelta per una chiave di entità non garantisce tuttavia in modo assoluto chiavi di entità univoche in un set di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-599">However, this choice for an entity key does not absolutely guarantee unique entity keys in an entity set.</span></span> <span data-ttu-id="cb8fc-600">In questo caso, è consigliabile aggiungere una proprietà, ad esempio **autorizzazione**, che può essere usata per identificare in modo univoco un autore.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-600">Adding a property, such as **AuthorId**, that could be used to uniquely identify an author would be recommended in this case.</span></span>

 

## <a name="member-element-csdl"></a><span data-ttu-id="cb8fc-601">Elemento member (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-601">Member Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-602">L'elemento **member** è un elemento figlio dell'elemento enumType e definisce un membro del tipo enumerato.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-602">The **Member** element is a child element of the EnumType element and defines a member of the enumerated type.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-603">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-603">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-604">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-604">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="cb8fc-605">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="cb8fc-605">Attribute Name</span></span> | <span data-ttu-id="cb8fc-606">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-606">Is Required</span></span> | <span data-ttu-id="cb8fc-607">Value</span><span class="sxs-lookup"><span data-stu-id="cb8fc-607">Value</span></span>                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cb8fc-608">**Name**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-608">**Name**</span></span>       | <span data-ttu-id="cb8fc-609">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-609">Yes</span></span>         | <span data-ttu-id="cb8fc-610">Nome del membro.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-610">The name of the member.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="cb8fc-611">**Valore**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-611">**Value**</span></span>      | <span data-ttu-id="cb8fc-612">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-612">No</span></span>          | <span data-ttu-id="cb8fc-613">Valore del membro.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-613">The value of the member.</span></span> <span data-ttu-id="cb8fc-614">Per impostazione predefinita, il primo membro ha il valore 0 e il valore di ogni enumeratore successivo viene incrementato di 1.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-614">By default, the first member has the value 0, and the value of each successive enumerator is incremented by 1.</span></span> <span data-ttu-id="cb8fc-615">Potrebbero esistere più membri con gli stessi valori.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-615">Multiple members with the same values may exist.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="cb8fc-616">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-616">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="cb8fc-617">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-617">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-618">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-618">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="cb8fc-619">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-619">Example</span></span>

<span data-ttu-id="cb8fc-620">Nell'esempio seguente viene illustrato un elemento **enumType** con tre elementi **member** :</span><span class="sxs-lookup"><span data-stu-id="cb8fc-620">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a><span data-ttu-id="cb8fc-621">Elemento NavigationProperty (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-621">NavigationProperty Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-622">Un elemento **NavigationProperty** definisce una proprietà di navigazione che fornisce un riferimento all'altra entità finale di un'associazione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-622">A **NavigationProperty** element defines a navigation property, which provides a reference to the other end of an association.</span></span> <span data-ttu-id="cb8fc-623">Diversamente dalle proprietà definite con l'elemento Property, le proprietà di navigazione non definiscono la forma e le caratteristiche dei dati.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-623">Unlike properties defined with the Property element, navigation properties do not define the shape and characteristics of data.</span></span> <span data-ttu-id="cb8fc-624">Forniscono un modo per navigare in un'associazione tra due tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-624">They provide a way to navigate an association between two entity types.</span></span>

<span data-ttu-id="cb8fc-625">Si noti che le proprietà di navigazione sono facoltative in entrambi tipi di entità nelle entità finali di un'associazione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-625">Note that navigation properties are optional on both entity types at the ends of an association.</span></span> <span data-ttu-id="cb8fc-626">Se si definisce una proprietà di navigazione su un tipo di entità nell'entità finale di un'associazione, non è necessario definire una proprietà di navigazione sul tipo di entità nell'altra entità finale dell'associazione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-626">If you define a navigation property on one entity type at the end of an association, you do not have to define a navigation property on the entity type at the other end of the association.</span></span>

<span data-ttu-id="cb8fc-627">Il tipo di dati restituito da una proprietà di navigazione è determinato dalla molteplicità della relativa entità finale di associazione remota.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-627">The data type returned by a navigation property is determined by the multiplicity of its remote association end.</span></span> <span data-ttu-id="cb8fc-628">Si supponga, ad esempio, che una proprietà di navigazione, **OrdersNavProp**, esista in un tipo di entità **Customer** e si sposta in un'associazione uno-a-molti tra **Customer** e **Order**.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-628">For example, suppose a navigation property, **OrdersNavProp**, exists on a **Customer** entity type and navigates a one-to-many association between **Customer** and **Order**.</span></span> <span data-ttu-id="cb8fc-629">Poiché l'entità finale dell'associazione remota per la proprietà di navigazione dispone di molteplicità many (\*), il relativo tipo di dati è una raccolta (di **Order**).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-629">Because the remote association end for the navigation property has multiplicity many (\*), its data type is a collection (of **Order**).</span></span> <span data-ttu-id="cb8fc-630">Analogamente, se esiste una proprietà di navigazione, **CustomerNavProp nel**, nel tipo di entità **Order** , il relativo tipo di dati sarà **Customer** poiché la molteplicità di end remoto è uno (1).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-630">Similarly, if a navigation property, **CustomerNavProp**, exists on the **Order** entity type, its data type would be **Customer** since the multiplicity of the remote end is one (1).</span></span>

<span data-ttu-id="cb8fc-631">Un elemento **NavigationProperty** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="cb8fc-631">A **NavigationProperty** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cb8fc-632">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-632">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="cb8fc-633">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-633">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-634">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-634">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-635">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **NavigationProperty** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-635">The table below describes the attributes that can be applied to the **NavigationProperty** element.</span></span>

| <span data-ttu-id="cb8fc-636">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="cb8fc-636">Attribute Name</span></span>   | <span data-ttu-id="cb8fc-637">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-637">Is Required</span></span> | <span data-ttu-id="cb8fc-638">Value</span><span class="sxs-lookup"><span data-stu-id="cb8fc-638">Value</span></span>                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cb8fc-639">**Name**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-639">**Name**</span></span>         | <span data-ttu-id="cb8fc-640">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-640">Yes</span></span>         | <span data-ttu-id="cb8fc-641">Nome della proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-641">The name of the navigation property.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="cb8fc-642">**Relazione**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-642">**Relationship**</span></span> | <span data-ttu-id="cb8fc-643">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-643">Yes</span></span>         | <span data-ttu-id="cb8fc-644">Nome di un'associazione che è all'interno dell'ambito del modello.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-644">The name of an association that is within the scope of the model.</span></span>                                                                                                                                                                                |
| <span data-ttu-id="cb8fc-645">**ToRole**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-645">**ToRole**</span></span>       | <span data-ttu-id="cb8fc-646">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-646">Yes</span></span>         | <span data-ttu-id="cb8fc-647">Entità finale dell'associazione in corrispondenza della quale termina la navigazione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-647">The end of the association at which navigation ends.</span></span> <span data-ttu-id="cb8fc-648">Il valore dell'attributo **ToRole** deve corrispondere al valore di uno degli attributi **Role** definiti in una delle entità finali dell'associazione (definite nell'elemento AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-648">The value of the **ToRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span>       |
| <span data-ttu-id="cb8fc-649">**FromRole**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-649">**FromRole**</span></span>     | <span data-ttu-id="cb8fc-650">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-650">Yes</span></span>         | <span data-ttu-id="cb8fc-651">Entità finale dell'associazione dalla quale ha inizio la navigazione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-651">The end of the association from which navigation begins.</span></span> <span data-ttu-id="cb8fc-652">Il valore dell'attributo **FromRole** deve corrispondere al valore di uno degli attributi **Role** definiti in una delle entità finali dell'associazione (definite nell'elemento AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-652">The value of the **FromRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="cb8fc-653">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **NavigationProperty** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-653">Any number of annotation attributes (custom XML attributes) may be applied to the **NavigationProperty** element.</span></span> <span data-ttu-id="cb8fc-654">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-654">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-655">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-655">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="cb8fc-656">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-656">Example</span></span>

<span data-ttu-id="cb8fc-657">Nell'esempio seguente viene definito un tipo di entità (**book**) con due proprietà di navigazione (**PublishedBy** e **WrittenBy**):</span><span class="sxs-lookup"><span data-stu-id="cb8fc-657">The following example defines an entity type (**Book**) with two navigation properties (**PublishedBy** and **WrittenBy**):</span></span>

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
 

 

## <a name="ondelete-element-csdl"></a><span data-ttu-id="cb8fc-658">Elemento OnDelete (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-658">OnDelete Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-659">L'elemento **OnDelete** in Conceptual Schema Definition Language (CSDL) definisce il comportamento connesso a un'associazione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-659">The **OnDelete** element in conceptual schema definition language (CSDL) defines behavior that is connected with an association.</span></span> <span data-ttu-id="cb8fc-660">Se l'attributo **Action** è impostato su **Cascade** in un'entità finale di un'associazione, i tipi di entità correlati sull'altra entità finale dell'associazione vengono eliminati quando viene eliminato il tipo di entità alla prima estremità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-660">If the **Action** attribute is set to **Cascade** on one end of an association, related entity types on the other end of the association are deleted when the entity type on the first end is deleted.</span></span> <span data-ttu-id="cb8fc-661">Se l'associazione tra due tipi di entità è una relazione tra chiave primaria e chiave primaria, un oggetto dipendente caricato viene eliminato quando l'oggetto Principal sull'altra entità finale dell'associazione viene eliminato indipendentemente dalla specifica **OnDelete** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-661">If the association between two entity types is a primary key-to-primary key relationship, then a loaded dependent object is deleted when the principal object on the other end of the association is deleted regardless of the **OnDelete** specification.</span></span>  

> [!NOTE]
> <span data-ttu-id="cb8fc-662">L'elemento **OnDelete** influisca solo sul comportamento di runtime di un'applicazione. non influisce sul comportamento nell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-662">The **OnDelete** element only affects the runtime behavior of an application; it does not affect behavior in the data source.</span></span> <span data-ttu-id="cb8fc-663">Il comportamento definito nell'origine dati deve essere uguale al comportamento definito nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-663">The behavior defined in the data source should be the same as the behavior defined in the application.</span></span>

 

<span data-ttu-id="cb8fc-664">Un elemento **OnDelete** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="cb8fc-664">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cb8fc-665">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-665">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="cb8fc-666">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-666">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-667">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-667">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-668">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **OnDelete** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-668">The table below describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="cb8fc-669">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="cb8fc-669">Attribute Name</span></span> | <span data-ttu-id="cb8fc-670">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-670">Is Required</span></span> | <span data-ttu-id="cb8fc-671">Value</span><span class="sxs-lookup"><span data-stu-id="cb8fc-671">Value</span></span>                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cb8fc-672">**Azione**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-672">**Action**</span></span>     | <span data-ttu-id="cb8fc-673">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-673">Yes</span></span>         | <span data-ttu-id="cb8fc-674">**Cascade** o **None**.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-674">**Cascade** or **None**.</span></span> <span data-ttu-id="cb8fc-675">Se **Cascade**, i tipi di entità dipendenti verranno eliminati quando il tipo di entità principale viene eliminato.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-675">If **Cascade**, dependent entity types will be deleted when the principal entity type is deleted.</span></span> <span data-ttu-id="cb8fc-676">Se **None**, i tipi di entità dipendenti non verranno eliminati quando il tipo di entità principale viene eliminato.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-676">If **None**, dependent entity types will not be deleted when the principal entity type is deleted.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="cb8fc-677">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-677">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="cb8fc-678">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-678">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-679">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-679">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="cb8fc-680">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-680">Example</span></span>

<span data-ttu-id="cb8fc-681">Nell'esempio seguente viene illustrato un elemento **Association** che definisce l'associazione **CustomerOrders** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-681">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="cb8fc-682">L'elemento **OnDelete** indica che tutti **gli ordini** relativi a un determinato **cliente** e che sono stati caricati in ObjectContext verranno eliminati quando il **cliente** viene eliminato.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-682">The **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted when the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a><span data-ttu-id="cb8fc-683">Elemento Parameter (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-683">Parameter Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-684">L'elemento **Parameter** in Conceptual Schema Definition Language (CSDL) può essere un elemento figlio dell'elemento FunctionImport o dell'elemento Function.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-684">The **Parameter** element in conceptual schema definition language (CSDL) can be a child of the FunctionImport element or the Function element.</span></span>

### <a name="functionimport-element-application"></a><span data-ttu-id="cb8fc-685">Applicazione dell'elemento FunctionImport</span><span class="sxs-lookup"><span data-stu-id="cb8fc-685">FunctionImport Element Application</span></span>

<span data-ttu-id="cb8fc-686">Un elemento **Parameter** (come figlio dell'elemento **FunctionImport** ) viene usato per definire i parametri di input e output per le importazioni di funzioni dichiarate in CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-686">A **Parameter** element (as a child of the **FunctionImport** element) is used to define input and output parameters for function imports that are declared in CSDL.</span></span>

<span data-ttu-id="cb8fc-687">L'elemento **Parameter** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="cb8fc-687">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cb8fc-688">Documentazione (zero o un elemento consentito)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-688">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="cb8fc-689">Elementi Annotation (zero o più elementi consentiti)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-689">Annotation elements (zero or more elements allowed)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-690">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-690">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-691">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-691">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="cb8fc-692">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="cb8fc-692">Attribute Name</span></span> | <span data-ttu-id="cb8fc-693">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-693">Is Required</span></span> | <span data-ttu-id="cb8fc-694">Value</span><span class="sxs-lookup"><span data-stu-id="cb8fc-694">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cb8fc-695">**Name**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-695">**Name**</span></span>       | <span data-ttu-id="cb8fc-696">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-696">Yes</span></span>         | <span data-ttu-id="cb8fc-697">Nome del parametro.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-697">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="cb8fc-698">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-698">**Type**</span></span>       | <span data-ttu-id="cb8fc-699">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-699">Yes</span></span>         | <span data-ttu-id="cb8fc-700">Tipo del parametro.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-700">The parameter type.</span></span> <span data-ttu-id="cb8fc-701">Il valore deve essere un **EDMSimpleType** o un tipo complesso che rientra nell'ambito del modello.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-701">The value must be an **EDMSimpleType** or a complex type that is within the scope of the model.</span></span>                                                                                                             |
| <span data-ttu-id="cb8fc-702">**Modalità**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-702">**Mode**</span></span>       | <span data-ttu-id="cb8fc-703">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-703">No</span></span>          | <span data-ttu-id="cb8fc-704">**In**, **out**o **InOut** a seconda che il parametro sia un parametro di input, di output o di input/output.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-704">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="cb8fc-705">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-705">**MaxLength**</span></span>  | <span data-ttu-id="cb8fc-706">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-706">No</span></span>          | <span data-ttu-id="cb8fc-707">Lunghezza massima consentita del parametro.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-707">The maximum allowed length of the parameter.</span></span>                                                                                                                                                                                    |
| <span data-ttu-id="cb8fc-708">**Precisione**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-708">**Precision**</span></span>  | <span data-ttu-id="cb8fc-709">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-709">No</span></span>          | <span data-ttu-id="cb8fc-710">Precisione del parametro.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-710">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="cb8fc-711">**Scala**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-711">**Scale**</span></span>      | <span data-ttu-id="cb8fc-712">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-712">No</span></span>          | <span data-ttu-id="cb8fc-713">Scala del parametro.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-713">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="cb8fc-714">**SRID**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-714">**SRID**</span></span>       | <span data-ttu-id="cb8fc-715">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-715">No</span></span>          | <span data-ttu-id="cb8fc-716">Identificatore di riferimento del sistema spaziale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-716">Spatial System Reference Identifier.</span></span> <span data-ttu-id="cb8fc-717">Valido solo per i parametri dei tipi spaziali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-717">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="cb8fc-718">Per ulteriori informazioni, vedere [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-718">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="cb8fc-719">All'elemento **Parameter** è possibile applicare qualsiasi numero di attributi di annotazione (attributi XML personalizzati).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-719">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="cb8fc-720">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-720">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-721">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-721">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="cb8fc-722">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-722">Example</span></span>

<span data-ttu-id="cb8fc-723">Nell'esempio seguente viene illustrato un elemento **FunctionImport** con un elemento figlio **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-723">The following example shows a **FunctionImport** element with one **Parameter** child element.</span></span> <span data-ttu-id="cb8fc-724">La funzione accetta un parametro di input e restituisce una raccolta di tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-724">The function accepts one input parameter and returns a collection of entity types.</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a><span data-ttu-id="cb8fc-725">Applicazione dell'elemento Function</span><span class="sxs-lookup"><span data-stu-id="cb8fc-725">Function Element Application</span></span>

<span data-ttu-id="cb8fc-726">Un elemento **Parameter** (come figlio dell'elemento **Function** ) definisce i parametri per le funzioni definite o dichiarate in un modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-726">A **Parameter** element (as a child of the **Function** element) defines parameters for functions that are defined or declared in a conceptual model.</span></span>

<span data-ttu-id="cb8fc-727">L'elemento **Parameter** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="cb8fc-727">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cb8fc-728">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-728">Documentation (zero or one elements)</span></span>
-   <span data-ttu-id="cb8fc-729">CollectionType (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-729">CollectionType (zero or one elements)</span></span>
-   <span data-ttu-id="cb8fc-730">ReferenceType (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-730">ReferenceType (zero or one elements)</span></span>
-   <span data-ttu-id="cb8fc-731">RowType (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-731">RowType (zero or one elements)</span></span>

> [!NOTE]
> <span data-ttu-id="cb8fc-732">Solo uno degli elementi **CollectionType**, **ReferenceType**o **RowType** può essere un elemento figlio di un elemento **Property** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-732">Only one of the **CollectionType**, **ReferenceType**, or **RowType** elements can be a child element of a **Property** element.</span></span>

 

-   <span data-ttu-id="cb8fc-733">Elementi Annotation (zero o più elementi consentiti)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-733">Annotation elements (zero or more elements allowed)</span></span>

> [!NOTE]
> <span data-ttu-id="cb8fc-734">Gli elementi Annotation devono apparire dopo tutti gli altri elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-734">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="cb8fc-735">Gli elementi Annotation sono consentiti solo in CSDL V2 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-735">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-736">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-736">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-737">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-737">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="cb8fc-738">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="cb8fc-738">Attribute Name</span></span>   | <span data-ttu-id="cb8fc-739">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-739">Is Required</span></span> | <span data-ttu-id="cb8fc-740">Value</span><span class="sxs-lookup"><span data-stu-id="cb8fc-740">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cb8fc-741">**Name**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-741">**Name**</span></span>         | <span data-ttu-id="cb8fc-742">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-742">Yes</span></span>         | <span data-ttu-id="cb8fc-743">Nome del parametro.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-743">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="cb8fc-744">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-744">**Type**</span></span>         | <span data-ttu-id="cb8fc-745">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-745">No</span></span>          | <span data-ttu-id="cb8fc-746">Tipo del parametro.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-746">The parameter type.</span></span> <span data-ttu-id="cb8fc-747">Un parametro può essere uno qualsiasi dei seguenti tipi (o raccolte di questi tipi):</span><span class="sxs-lookup"><span data-stu-id="cb8fc-747">A parameter can be any of the following types (or collections of these types):</span></span> <br/> <span data-ttu-id="cb8fc-748">**EdmSimpleType**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-748">**EdmSimpleType**</span></span> <br/> <span data-ttu-id="cb8fc-749">tipo di entità</span><span class="sxs-lookup"><span data-stu-id="cb8fc-749">entity type</span></span> <br/> <span data-ttu-id="cb8fc-750">tipo complesso</span><span class="sxs-lookup"><span data-stu-id="cb8fc-750">complex type</span></span> <br/> <span data-ttu-id="cb8fc-751">tipo di riga</span><span class="sxs-lookup"><span data-stu-id="cb8fc-751">row type</span></span> <br/> <span data-ttu-id="cb8fc-752">tipo di riferimento</span><span class="sxs-lookup"><span data-stu-id="cb8fc-752">reference type</span></span>                             |
| <span data-ttu-id="cb8fc-753">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-753">**Nullable**</span></span>     | <span data-ttu-id="cb8fc-754">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-754">No</span></span>          | <span data-ttu-id="cb8fc-755">**True** (valore predefinito) o **false** a seconda che la proprietà possa avere un valore **null** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-755">**True** (the default value) or **False** depending on whether the property can have a **null** value.</span></span>                                                                                                                          |
| <span data-ttu-id="cb8fc-756">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-756">**DefaultValue**</span></span> | <span data-ttu-id="cb8fc-757">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-757">No</span></span>          | <span data-ttu-id="cb8fc-758">Valore predefinito della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-758">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="cb8fc-759">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-759">**MaxLength**</span></span>    | <span data-ttu-id="cb8fc-760">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-760">No</span></span>          | <span data-ttu-id="cb8fc-761">Lunghezza massima del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-761">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="cb8fc-762">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-762">**FixedLength**</span></span>  | <span data-ttu-id="cb8fc-763">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-763">No</span></span>          | <span data-ttu-id="cb8fc-764">**True** o **false** a seconda che il valore della proprietà venga archiviato come stringa a lunghezza fissa.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-764">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="cb8fc-765">**Precisione**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-765">**Precision**</span></span>    | <span data-ttu-id="cb8fc-766">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-766">No</span></span>          | <span data-ttu-id="cb8fc-767">Precisione del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-767">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="cb8fc-768">**Scala**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-768">**Scale**</span></span>        | <span data-ttu-id="cb8fc-769">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-769">No</span></span>          | <span data-ttu-id="cb8fc-770">Scala del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-770">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="cb8fc-771">**SRID**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-771">**SRID**</span></span>         | <span data-ttu-id="cb8fc-772">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-772">No</span></span>          | <span data-ttu-id="cb8fc-773">Identificatore di riferimento del sistema spaziale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-773">Spatial System Reference Identifier.</span></span> <span data-ttu-id="cb8fc-774">Valido solo per le proprietà dei tipi spaziali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-774">Valid only for properties of spatial types.</span></span> <span data-ttu-id="cb8fc-775">Per ulteriori informazioni, vedere [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-775">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="cb8fc-776">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-776">**Unicode**</span></span>      | <span data-ttu-id="cb8fc-777">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-777">No</span></span>          | <span data-ttu-id="cb8fc-778">**True** o **false** a seconda che il valore della proprietà venga archiviato come stringa Unicode.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-778">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="cb8fc-779">**Confronto**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-779">**Collation**</span></span>    | <span data-ttu-id="cb8fc-780">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-780">No</span></span>          | <span data-ttu-id="cb8fc-781">Stringa che specifica la sequenza di confronto da utilizzare nell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-781">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="cb8fc-782">All'elemento **Parameter** è possibile applicare qualsiasi numero di attributi di annotazione (attributi XML personalizzati).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-782">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="cb8fc-783">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-783">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-784">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-784">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="cb8fc-785">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-785">Example</span></span>

<span data-ttu-id="cb8fc-786">Nell'esempio seguente viene illustrato un elemento **Function** che utilizza un elemento figlio **Parameter** per definire un parametro di funzione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-786">The following example shows a **Function** element that uses one **Parameter** child element to define a function parameter.</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```

 

## <a name="principal-element-csdl"></a><span data-ttu-id="cb8fc-787">Elemento Principal (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-787">Principal Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-788">L'elemento **Principal** in Conceptual Schema Definition Language (CSDL) è un elemento figlio dell'elemento ReferentialConstraint che definisce l'entità finale principale di un vincolo referenziale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-788">The **Principal** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element that defines the principal end of a referential constraint.</span></span> <span data-ttu-id="cb8fc-789">Un elemento **ReferentialConstraint** definisce la funzionalità che è simile a un vincolo di integrità referenziale in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-789">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="cb8fc-790">Nello stesso modo in cui una colonna (o più colonne) da una tabella di database può fare riferimento alla chiave primaria di un'altra tabella, una proprietà (o più proprietà) di un tipo di entità può fare riferimento alla chiave di entità di un altro tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-790">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="cb8fc-791">Il tipo di entità a cui si fa riferimento viene chiamato *entità finale principale* del vincolo.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-791">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="cb8fc-792">Il tipo di entità che fa riferimento all'entità finale principale viene chiamato entità *finale dipendente* del vincolo.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-792">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="cb8fc-793">Gli elementi **PropertyRef** vengono utilizzati per specificare a quali chiavi viene fatto riferimento dall'entità finale dipendente.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-793">**PropertyRef** elements are used to specify which keys are referenced by the dependent end.</span></span>

<span data-ttu-id="cb8fc-794">L'elemento **Principal** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="cb8fc-794">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cb8fc-795">PropertyRef (uno o più elementi)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-795">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="cb8fc-796">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-796">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-797">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-797">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-798">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **Principal** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-798">The table below describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="cb8fc-799">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="cb8fc-799">Attribute Name</span></span> | <span data-ttu-id="cb8fc-800">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-800">Is Required</span></span> | <span data-ttu-id="cb8fc-801">Value</span><span class="sxs-lookup"><span data-stu-id="cb8fc-801">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="cb8fc-802">**Ruolo**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-802">**Role**</span></span>       | <span data-ttu-id="cb8fc-803">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-803">Yes</span></span>         | <span data-ttu-id="cb8fc-804">Nome del tipo di entità in un'entità finale principale dell'associazione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-804">The name of the entity type on the principal end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="cb8fc-805">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **principale** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-805">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="cb8fc-806">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-806">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-807">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-807">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="cb8fc-808">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-808">Example</span></span>

<span data-ttu-id="cb8fc-809">Nell'esempio seguente viene illustrato un elemento **ReferentialConstraint** che fa parte della definizione dell'associazione **PublishedBy** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-809">The following example shows a **ReferentialConstraint** element that is part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="cb8fc-810">La proprietà **ID** del tipo di entità **Publisher** costituisce l'entità finale principale del vincolo referenziale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-810">The **Id** property of the **Publisher** entity type makes up the principal end of the referential constraint.</span></span>

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
 

 

## <a name="property-element-csdl"></a><span data-ttu-id="cb8fc-811">Elemento Property (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-811">Property Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-812">L'elemento **Property** in Conceptual Schema Definition Language (CSDL) può essere un elemento figlio dell'elemento EntityType, dell'elemento complexType o dell'elemento RowType.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-812">The **Property** element in conceptual schema definition language (CSDL) can be a child of the EntityType element, the ComplexType element, or the RowType element.</span></span>

### <a name="entitytype-and-complextype-element-applications"></a><span data-ttu-id="cb8fc-813">Applicazione degli elementi EntityType e ComplexType</span><span class="sxs-lookup"><span data-stu-id="cb8fc-813">EntityType and ComplexType Element Applications</span></span>

<span data-ttu-id="cb8fc-814">Gli elementi **Property** (come elementi figlio degli elementi **EntityType** o **complexType** ) definiscono la forma e le caratteristiche dei dati che un'istanza del tipo di entità o di tipo complesso conterrà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-814">**Property** elements (as children of **EntityType** or **ComplexType** elements) define the shape and characteristics of data that an entity type instance or complex type instance will contain.</span></span> <span data-ttu-id="cb8fc-815">Le proprietà in un modello concettuale sono analoghe alle proprietà definite su una classe.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-815">Properties in a conceptual model are analogous to properties that are defined on a class.</span></span> <span data-ttu-id="cb8fc-816">Nello stesso modo in cui le proprietà su una classe definiscono la forma della classe e forniscono informazioni su oggetti, le proprietà in un modello concettuale definiscono la forma di un tipo di entità e forniscono informazioni su istanze del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-816">In the same way that properties on a class define the shape of the class and carry information about objects, properties in a conceptual model define the shape of an entity type and carry information about entity type instances.</span></span>

<span data-ttu-id="cb8fc-817">L'elemento **Property** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="cb8fc-817">The **Property** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cb8fc-818">Elemento Documentation (zero o uno degli elementi consentiti)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-818">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="cb8fc-819">Elementi Annotation (zero o più elementi consentiti)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-819">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="cb8fc-820">A un elemento **Property** è possibile applicare i facet seguenti: **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **scale**, **Unicode**, **Collation**, **ConcurrencyMode**.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-820">The following facets can be applied to a **Property** element: **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **Collation**, **ConcurrencyMode**.</span></span> <span data-ttu-id="cb8fc-821">I facet sono attributi XML che forniscono informazioni sul modo in cui i valori di proprietà vengono archiviati nell'archivio dati.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-821">Facets are XML attributes that provide information about how property values are stored in the data store.</span></span>

> [!NOTE]
> <span data-ttu-id="cb8fc-822">I facet possono essere applicati solo a proprietà di tipo **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-822">Facets can only be applied to properties of type **EDMSimpleType**.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-823">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-823">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-824">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **Property** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-824">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="cb8fc-825">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="cb8fc-825">Attribute Name</span></span>                                                         | <span data-ttu-id="cb8fc-826">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-826">Is Required</span></span> | <span data-ttu-id="cb8fc-827">Value</span><span class="sxs-lookup"><span data-stu-id="cb8fc-827">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cb8fc-828">**Name**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-828">**Name**</span></span>                                                               | <span data-ttu-id="cb8fc-829">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-829">Yes</span></span>         | <span data-ttu-id="cb8fc-830">Nome della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-830">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="cb8fc-831">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-831">**Type**</span></span>                                                               | <span data-ttu-id="cb8fc-832">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-832">Yes</span></span>         | <span data-ttu-id="cb8fc-833">Tipo del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-833">The type of the property value.</span></span> <span data-ttu-id="cb8fc-834">Il tipo di valore della proprietà deve essere un **EDMSimpleType** o un tipo complesso (indicato da un nome completo) che rientra nell'ambito del modello.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-834">The property value type must be an **EDMSimpleType** or a complex type (indicated by a fully-qualified name) that is within scope of the model.</span></span>                                                 |
| <span data-ttu-id="cb8fc-835">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-835">**Nullable**</span></span>                                                           | <span data-ttu-id="cb8fc-836">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-836">No</span></span>          | <span data-ttu-id="cb8fc-837">**True** (valore predefinito) o <strong>false</strong> a seconda che la proprietà possa avere un valore null.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-837">**True** (the default value) or <strong>False</strong> depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                   |
| <span data-ttu-id="cb8fc-838">> In CSDL V1, una proprietà di tipo complesso deve avere `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-838">> In the CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="cb8fc-839">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-839">**DefaultValue**</span></span>                                                       | <span data-ttu-id="cb8fc-840">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-840">No</span></span>          | <span data-ttu-id="cb8fc-841">Valore predefinito della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-841">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="cb8fc-842">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-842">**MaxLength**</span></span>                                                          | <span data-ttu-id="cb8fc-843">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-843">No</span></span>          | <span data-ttu-id="cb8fc-844">Lunghezza massima del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-844">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="cb8fc-845">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-845">**FixedLength**</span></span>                                                        | <span data-ttu-id="cb8fc-846">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-846">No</span></span>          | <span data-ttu-id="cb8fc-847">**True** o **false** a seconda che il valore della proprietà venga archiviato come stringa a lunghezza fissa.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-847">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="cb8fc-848">**Precisione**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-848">**Precision**</span></span>                                                          | <span data-ttu-id="cb8fc-849">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-849">No</span></span>          | <span data-ttu-id="cb8fc-850">Precisione del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-850">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="cb8fc-851">**Scala**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-851">**Scale**</span></span>                                                              | <span data-ttu-id="cb8fc-852">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-852">No</span></span>          | <span data-ttu-id="cb8fc-853">Scala del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-853">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="cb8fc-854">**SRID**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-854">**SRID**</span></span>                                                               | <span data-ttu-id="cb8fc-855">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-855">No</span></span>          | <span data-ttu-id="cb8fc-856">Identificatore di riferimento del sistema spaziale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-856">Spatial System Reference Identifier.</span></span> <span data-ttu-id="cb8fc-857">Valido solo per le proprietà dei tipi spaziali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-857">Valid only for properties of spatial types.</span></span> <span data-ttu-id="cb8fc-858">Per ulteriori informazioni, vedere [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-858">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="cb8fc-859">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-859">**Unicode**</span></span>                                                            | <span data-ttu-id="cb8fc-860">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-860">No</span></span>          | <span data-ttu-id="cb8fc-861">**True** o **false** a seconda che il valore della proprietà venga archiviato come stringa Unicode.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-861">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="cb8fc-862">**Confronto**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-862">**Collation**</span></span>                                                          | <span data-ttu-id="cb8fc-863">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-863">No</span></span>          | <span data-ttu-id="cb8fc-864">Stringa che specifica la sequenza di confronto da utilizzare nell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-864">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="cb8fc-865">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-865">**ConcurrencyMode**</span></span>                                                    | <span data-ttu-id="cb8fc-866">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-866">No</span></span>          | <span data-ttu-id="cb8fc-867">**None** (valore predefinito) o **fixed**.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-867">**None** (the default value) or **Fixed**.</span></span> <span data-ttu-id="cb8fc-868">Se il valore è impostato su **fixed**, il valore della proprietà verrà utilizzato nei controlli di concorrenza ottimistica.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-868">If the value is set to **Fixed**, the property value will be used in optimistic concurrency checks.</span></span>                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="cb8fc-869">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **Property** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-869">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="cb8fc-870">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-870">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-871">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-871">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="cb8fc-872">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-872">Example</span></span>

<span data-ttu-id="cb8fc-873">Nell'esempio seguente viene illustrato un elemento **EntityType** con tre elementi **Property** :</span><span class="sxs-lookup"><span data-stu-id="cb8fc-873">The following example shows an **EntityType** element with three **Property** elements:</span></span>

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
 

<span data-ttu-id="cb8fc-874">Nell'esempio seguente viene illustrato un elemento **complexType** con cinque elementi **Property** :</span><span class="sxs-lookup"><span data-stu-id="cb8fc-874">The following example shows a **ComplexType** element with five **Property** elements:</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a><span data-ttu-id="cb8fc-875">Applicazione dell'elemento RowType</span><span class="sxs-lookup"><span data-stu-id="cb8fc-875">RowType Element Application</span></span>

<span data-ttu-id="cb8fc-876">Gli elementi **Property** (come gli elementi figlio di un elemento **RowType** ) definiscono la forma e le caratteristiche dei dati che possono essere passati o restituiti da una funzione definita dal modello.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-876">**Property** elements (as the children of a **RowType** element) define the shape and characteristics of data that can be passed to or returned from a model-defined function.</span></span>  

<span data-ttu-id="cb8fc-877">L'elemento **Property** può avere esattamente uno degli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="cb8fc-877">The **Property** element can have exactly one of the following child elements:</span></span>

-   <span data-ttu-id="cb8fc-878">CollectionType</span><span class="sxs-lookup"><span data-stu-id="cb8fc-878">CollectionType</span></span>
-   <span data-ttu-id="cb8fc-879">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="cb8fc-879">ReferenceType</span></span>
-   <span data-ttu-id="cb8fc-880">RowType</span><span class="sxs-lookup"><span data-stu-id="cb8fc-880">RowType</span></span>

<span data-ttu-id="cb8fc-881">L'elemento **Property** può includere qualsiasi numero di elementi Annotation figlio.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-881">The **Property** element can have any number child annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="cb8fc-882">Gli elementi Annotation sono consentiti solo in CSDL V2 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-882">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-883">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-883">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-884">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **Property** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-884">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="cb8fc-885">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="cb8fc-885">Attribute Name</span></span>                                                     | <span data-ttu-id="cb8fc-886">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-886">Is Required</span></span> | <span data-ttu-id="cb8fc-887">Value</span><span class="sxs-lookup"><span data-stu-id="cb8fc-887">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cb8fc-888">**Name**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-888">**Name**</span></span>                                                           | <span data-ttu-id="cb8fc-889">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-889">Yes</span></span>         | <span data-ttu-id="cb8fc-890">Nome della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-890">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="cb8fc-891">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-891">**Type**</span></span>                                                           | <span data-ttu-id="cb8fc-892">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-892">Yes</span></span>         | <span data-ttu-id="cb8fc-893">Tipo del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-893">The type of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="cb8fc-894">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-894">**Nullable**</span></span>                                                       | <span data-ttu-id="cb8fc-895">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-895">No</span></span>          | <span data-ttu-id="cb8fc-896">**True** (valore predefinito) o **false** a seconda che la proprietà possa avere un valore null.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-896">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="cb8fc-897">> In CSDL V1 una proprietà di tipo complesso deve avere `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-897">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="cb8fc-898">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-898">**DefaultValue**</span></span>                                                   | <span data-ttu-id="cb8fc-899">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-899">No</span></span>          | <span data-ttu-id="cb8fc-900">Valore predefinito della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-900">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="cb8fc-901">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-901">**MaxLength**</span></span>                                                      | <span data-ttu-id="cb8fc-902">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-902">No</span></span>          | <span data-ttu-id="cb8fc-903">Lunghezza massima del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-903">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="cb8fc-904">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-904">**FixedLength**</span></span>                                                    | <span data-ttu-id="cb8fc-905">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-905">No</span></span>          | <span data-ttu-id="cb8fc-906">**True** o **false** a seconda che il valore della proprietà venga archiviato come stringa a lunghezza fissa.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-906">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="cb8fc-907">**Precisione**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-907">**Precision**</span></span>                                                      | <span data-ttu-id="cb8fc-908">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-908">No</span></span>          | <span data-ttu-id="cb8fc-909">Precisione del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-909">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="cb8fc-910">**Scala**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-910">**Scale**</span></span>                                                          | <span data-ttu-id="cb8fc-911">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-911">No</span></span>          | <span data-ttu-id="cb8fc-912">Scala del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-912">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="cb8fc-913">**SRID**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-913">**SRID**</span></span>                                                           | <span data-ttu-id="cb8fc-914">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-914">No</span></span>          | <span data-ttu-id="cb8fc-915">Identificatore di riferimento del sistema spaziale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-915">Spatial System Reference Identifier.</span></span> <span data-ttu-id="cb8fc-916">Valido solo per le proprietà dei tipi spaziali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-916">Valid only for properties of spatial types.</span></span> <span data-ttu-id="cb8fc-917">Per ulteriori informazioni, vedere [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-917">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="cb8fc-918">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-918">**Unicode**</span></span>                                                        | <span data-ttu-id="cb8fc-919">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-919">No</span></span>          | <span data-ttu-id="cb8fc-920">**True** o **false** a seconda che il valore della proprietà venga archiviato come stringa Unicode.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-920">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="cb8fc-921">**Confronto**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-921">**Collation**</span></span>                                                      | <span data-ttu-id="cb8fc-922">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-922">No</span></span>          | <span data-ttu-id="cb8fc-923">Stringa che specifica la sequenza di confronto da utilizzare nell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-923">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="cb8fc-924">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **Property** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-924">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="cb8fc-925">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-925">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-926">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-926">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="cb8fc-927">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-927">Example</span></span>

<span data-ttu-id="cb8fc-928">Nell'esempio seguente vengono illustrati gli elementi di **Proprietà** utilizzati per definire la forma del tipo restituito di una funzione definita dal modello.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-928">The following example shows **Property** elements used to define the shape of the return type of a model-defined function.</span></span>

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
 

 

## <a name="propertyref-element-csdl"></a><span data-ttu-id="cb8fc-929">Elemento PropertyRef (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-929">PropertyRef Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-930">L'elemento **PropertyRef** in Conceptual Schema Definition Language (CSDL) fa riferimento a una proprietà di un tipo di entità per indicare che la proprietà eseguirà uno dei ruoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="cb8fc-930">The **PropertyRef** element in conceptual schema definition language (CSDL) references a property of an entity type to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="cb8fc-931">Parte della chiave di entità (una proprietà o un set di proprietà di un tipo di entità che determinano l'identità).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-931">Part of the entity's key (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="cb8fc-932">Per definire una chiave di entità, è possibile usare uno o più elementi **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-932">One or more **PropertyRef** elements can be used to define an entity key.</span></span>
-   <span data-ttu-id="cb8fc-933">L'entità finale dipendente o principale di un vincolo referenziale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-933">The dependent or principal end of a referential constraint.</span></span>

<span data-ttu-id="cb8fc-934">L'elemento **PropertyRef** può contenere solo elementi Annotation (zero o più) come elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-934">The **PropertyRef** element can only have annotation elements (zero or more) as child elements.</span></span>

> [!NOTE]
> <span data-ttu-id="cb8fc-935">Gli elementi Annotation sono consentiti solo in CSDL V2 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-935">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-936">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-936">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-937">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-937">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="cb8fc-938">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="cb8fc-938">Attribute Name</span></span> | <span data-ttu-id="cb8fc-939">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-939">Is Required</span></span> | <span data-ttu-id="cb8fc-940">Value</span><span class="sxs-lookup"><span data-stu-id="cb8fc-940">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="cb8fc-941">**Name**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-941">**Name**</span></span>       | <span data-ttu-id="cb8fc-942">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-942">Yes</span></span>         | <span data-ttu-id="cb8fc-943">Nome della proprietà alla quale viene fatto riferimento.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-943">The name of the referenced property.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="cb8fc-944">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-944">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="cb8fc-945">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-945">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-946">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-946">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="cb8fc-947">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-947">Example</span></span>

<span data-ttu-id="cb8fc-948">Nell'esempio seguente viene definito un tipo di entità (**book**).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-948">The example below defines an entity type (**Book**).</span></span> <span data-ttu-id="cb8fc-949">La chiave di entità viene definita facendo riferimento alla proprietà **ISBN** del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-949">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="cb8fc-950">Nell'esempio seguente vengono usati due elementi **PropertyRef** per indicare che due proprietà (**ID** e **publisherID**) sono le estremità principale e dipendente di un vincolo referenziale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-950">In the next example, two **PropertyRef** elements are used to indicate that two properties (**Id** and **PublisherId**) are the principal and dependent ends of a referential constraint.</span></span>

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
 

 

## <a name="referencetype-element-csdl"></a><span data-ttu-id="cb8fc-951">Elemento ReferenceType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-951">ReferenceType Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-952">L'elemento **ReferenceType** in Conceptual Schema Definition Language (CSDL) specifica un riferimento a un tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-952">The **ReferenceType** element in conceptual schema definition language (CSDL) specifies a reference to an entity type.</span></span> <span data-ttu-id="cb8fc-953">L'elemento **ReferenceType** può essere figlio dei seguenti elementi:</span><span class="sxs-lookup"><span data-stu-id="cb8fc-953">The **ReferenceType** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="cb8fc-954">ReturnType (funzione)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-954">ReturnType (Function)</span></span>
-   <span data-ttu-id="cb8fc-955">Parametro</span><span class="sxs-lookup"><span data-stu-id="cb8fc-955">Parameter</span></span>
-   <span data-ttu-id="cb8fc-956">CollectionType</span><span class="sxs-lookup"><span data-stu-id="cb8fc-956">CollectionType</span></span>

<span data-ttu-id="cb8fc-957">L'elemento **ReferenceType** viene usato quando si definisce un parametro o un tipo restituito per una funzione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-957">The **ReferenceType** element is used when defining a parameter or return type for a function.</span></span>

<span data-ttu-id="cb8fc-958">Un elemento **ReferenceType** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="cb8fc-958">A **ReferenceType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cb8fc-959">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-959">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="cb8fc-960">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-960">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-961">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-961">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-962">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **ReferenceType** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-962">The table below describes the attributes that can be applied to the **ReferenceType** element.</span></span>

| <span data-ttu-id="cb8fc-963">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="cb8fc-963">Attribute Name</span></span> | <span data-ttu-id="cb8fc-964">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-964">Is Required</span></span> | <span data-ttu-id="cb8fc-965">Value</span><span class="sxs-lookup"><span data-stu-id="cb8fc-965">Value</span></span>                                         |
|:---------------|:------------|:----------------------------------------------|
| <span data-ttu-id="cb8fc-966">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-966">**Type**</span></span>       | <span data-ttu-id="cb8fc-967">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-967">Yes</span></span>         | <span data-ttu-id="cb8fc-968">Nome del tipo di entità a cui viene fatto riferimento.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-968">The name of the entity type being referenced.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="cb8fc-969">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **ReferenceType** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-969">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferenceType** element.</span></span> <span data-ttu-id="cb8fc-970">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-970">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-971">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-971">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="cb8fc-972">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-972">Example</span></span>

<span data-ttu-id="cb8fc-973">Nell'esempio seguente viene illustrato l'elemento **ReferenceType** usato come figlio di un elemento **Parameter** in una funzione definita dal modello che accetta un riferimento a un tipo di entità **Person** :</span><span class="sxs-lookup"><span data-stu-id="cb8fc-973">The following example shows the **ReferenceType** element used as a child of a **Parameter** element in a model-defined function that accepts a reference to a **Person** entity type:</span></span>

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
 

<span data-ttu-id="cb8fc-974">Nell'esempio seguente viene illustrato l'elemento **ReferenceType** utilizzato come elemento figlio di un elemento **returnType** (Function) in una funzione definita dal modello che restituisce un riferimento a un tipo di entità **Person** :</span><span class="sxs-lookup"><span data-stu-id="cb8fc-974">The following example shows the **ReferenceType** element used as a child of a **ReturnType** (Function) element in a model-defined function that returns a reference to a **Person** entity type:</span></span>

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
 

 

## <a name="referentialconstraint-element-csdl"></a><span data-ttu-id="cb8fc-975">Elemento ReferentialConstraint (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-975">ReferentialConstraint Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-976">Un elemento **ReferentialConstraint** in Conceptual Schema Definition Language (CSDL) definisce la funzionalità che è simile a un vincolo di integrità referenziale in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-976">A **ReferentialConstraint** element in conceptual schema definition language (CSDL) defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="cb8fc-977">Nello stesso modo in cui una colonna (o più colonne) da una tabella di database può fare riferimento alla chiave primaria di un'altra tabella, una proprietà (o più proprietà) di un tipo di entità può fare riferimento alla chiave di entità di un altro tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-977">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="cb8fc-978">Il tipo di entità a cui si fa riferimento viene chiamato *entità finale principale* del vincolo.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-978">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="cb8fc-979">Il tipo di entità che fa riferimento all'entità finale principale viene chiamato entità *finale dipendente* del vincolo.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-979">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span>

<span data-ttu-id="cb8fc-980">Se una chiave esterna esposta in un tipo di entità fa riferimento a una proprietà in un altro tipo di entità, l'elemento **ReferentialConstraint** definisce un'associazione tra i due tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-980">If a foreign key that is exposed on one entity type references a property on another entity type, the **ReferentialConstraint** element defines an association between the two entity types.</span></span> <span data-ttu-id="cb8fc-981">Poiché l'elemento **ReferentialConstraint** fornisce informazioni sul modo in cui due tipi di entità sono correlati, non è necessario alcun elemento AssociationSetMapping corrispondente nella Mapping Specification Language (MSL).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-981">Because the **ReferentialConstraint** element provides information about how two entity types are related, no corresponding AssociationSetMapping element is necessary in the mapping specification language (MSL).</span></span> <span data-ttu-id="cb8fc-982">Un'associazione tra due tipi di entità che non dispongono di chiavi esterne esposte deve avere un elemento **AssociationSetMapping** corrispondente per eseguire il mapping delle informazioni di associazione all'origine dati.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-982">An association between two entity types that do not have foreign keys exposed must have a corresponding **AssociationSetMapping** element in order to map association information to the data source.</span></span>

<span data-ttu-id="cb8fc-983">Se una chiave esterna non è esposta in un tipo di entità, l'elemento **ReferentialConstraint** può definire solo un vincolo PRIMARY KEY-Primary Key tra il tipo di entità e un altro tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-983">If a foreign key is not exposed on an entity type, the **ReferentialConstraint** element can only define a primary key-to-primary key constraint between the entity type and another entity type.</span></span>

<span data-ttu-id="cb8fc-984">Un elemento **ReferentialConstraint** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="cb8fc-984">A **ReferentialConstraint** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cb8fc-985">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-985">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="cb8fc-986">Principal (esattamente un elemento)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-986">Principal (exactly one element)</span></span>
-   <span data-ttu-id="cb8fc-987">Dependent (esattamente un elemento)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-987">Dependent (exactly one element)</span></span>
-   <span data-ttu-id="cb8fc-988">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-988">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-989">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-989">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-990">L'elemento **ReferentialConstraint** può avere un numero qualsiasi di attributi di annotazione (attributi XML personalizzati).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-990">The **ReferentialConstraint** element can have any number of annotation attributes (custom XML attributes).</span></span> <span data-ttu-id="cb8fc-991">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-991">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-992">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-992">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="cb8fc-993">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-993">Example</span></span>

<span data-ttu-id="cb8fc-994">Nell'esempio seguente viene illustrato un elemento **ReferentialConstraint** usato come parte della definizione dell'associazione **PublishedBy** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-994">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span>

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
 

 

## <a name="returntype-function-element-csdl"></a><span data-ttu-id="cb8fc-995">Elemento ReturnType (Function) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-995">ReturnType (Function) Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-996">L'elemento **returnType** (Function) in Conceptual Schema Definition Language (CSDL) specifica il tipo restituito per una funzione definita in un elemento Function.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-996">The **ReturnType** (Function) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a Function element.</span></span> <span data-ttu-id="cb8fc-997">Un tipo restituito di funzione può essere specificato anche con un attributo **returnType** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-997">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="cb8fc-998">I tipi restituiti possono essere qualsiasi **EDMSimpleType**, tipo di entità, tipo complesso, tipo di riga, tipo di riferimento o una raccolta di uno di questi tipi.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-998">Return types can be any **EdmSimpleType**, entity type, complex type, row type, ref type, or a collection of one of these types.</span></span>

<span data-ttu-id="cb8fc-999">Il tipo restituito di una funzione può essere specificato con l'attributo **Type** dell'elemento **returnType** (Function) o con uno degli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="cb8fc-999">The return type of a function can be specified with either the **Type** attribute of the **ReturnType** (Function) element, or with one of the following child elements:</span></span>

-   <span data-ttu-id="cb8fc-1000">CollectionType</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1000">CollectionType</span></span>
-   <span data-ttu-id="cb8fc-1001">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1001">ReferenceType</span></span>
-   <span data-ttu-id="cb8fc-1002">RowType</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1002">RowType</span></span>

> [!NOTE]
> <span data-ttu-id="cb8fc-1003">Un modello non viene convalidato se si specifica un tipo restituito dalla funzione con l'attributo **Type** dell'elemento **returnType** (Function) e uno degli elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1003">A model will not validate if you specify a function return type with both the **Type** attribute of the **ReturnType** (Function) element and one of the child elements.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-1004">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1004">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-1005">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **returnType** (Function).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1005">The following table describes the attributes that can be applied to the **ReturnType** (Function) element.</span></span>

| <span data-ttu-id="cb8fc-1006">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1006">Attribute Name</span></span> | <span data-ttu-id="cb8fc-1007">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1007">Is Required</span></span> | <span data-ttu-id="cb8fc-1008">Value</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1008">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="cb8fc-1009">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1009">**ReturnType**</span></span> | <span data-ttu-id="cb8fc-1010">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1010">No</span></span>          | <span data-ttu-id="cb8fc-1011">Tipo restituito dalla funzione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1011">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="cb8fc-1012">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **returnType** (Function).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1012">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (Function) element.</span></span> <span data-ttu-id="cb8fc-1013">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1013">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-1014">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1014">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="cb8fc-1015">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1015">Example</span></span>

<span data-ttu-id="cb8fc-1016">Nell'esempio seguente viene usato un elemento **Function** per definire una funzione che restituisce il numero di anni in cui un libro è stato stampato.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1016">The following example uses a **Function** element to define a function that returns the number of years a book has been in print.</span></span> <span data-ttu-id="cb8fc-1017">Si noti che il tipo restituito viene specificato dall'attributo **Type** di un elemento **returnType** (Function).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1017">Note that the return type is specified by the **Type** attribute of a **ReturnType** (Function) element.</span></span>

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a><span data-ttu-id="cb8fc-1018">Elemento ReturnType (FunctionImport) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1018">ReturnType (FunctionImport) Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-1019">L'elemento **returnType** (FunctionImport) in Conceptual Schema Definition Language (CSDL) specifica il tipo restituito per una funzione definita in un elemento FunctionImport.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1019">The **ReturnType** (FunctionImport) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a FunctionImport element.</span></span> <span data-ttu-id="cb8fc-1020">Un tipo restituito di funzione può essere specificato anche con un attributo **returnType** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1020">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="cb8fc-1021">I tipi restituiti possono essere qualsiasi raccolta di tipi di entità, tipi complessi o **EDMSimpleType**,</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1021">Return types can be any collection of entity type, complex type,or **EdmSimpleType**,</span></span>

<span data-ttu-id="cb8fc-1022">Il tipo restituito di una funzione viene specificato con l'attributo **Type** dell'elemento **returnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1022">The return type of a function is specified with the **Type** attribute of the **ReturnType** (FunctionImport) element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-1023">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1023">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-1024">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **returnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1024">The following table describes the attributes that can be applied to the **ReturnType** (FunctionImport) element.</span></span>

| <span data-ttu-id="cb8fc-1025">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1025">Attribute Name</span></span> | <span data-ttu-id="cb8fc-1026">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1026">Is Required</span></span> | <span data-ttu-id="cb8fc-1027">Value</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1027">Value</span></span>                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cb8fc-1028">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1028">**Type**</span></span>       | <span data-ttu-id="cb8fc-1029">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1029">No</span></span>          | <span data-ttu-id="cb8fc-1030">Tipo restituito dalla funzione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1030">The type that the function returns.</span></span> <span data-ttu-id="cb8fc-1031">Il valore deve essere una raccolta di ComplexType, EntityType o EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1031">The value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>                                                                                      |
| <span data-ttu-id="cb8fc-1032">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1032">**EntitySet**</span></span>  | <span data-ttu-id="cb8fc-1033">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1033">No</span></span>          | <span data-ttu-id="cb8fc-1034">Se la funzione restituisce una raccolta di tipi di entità, il valore di **EntitySet** deve essere il set di entità a cui appartiene la raccolta.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1034">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="cb8fc-1035">In caso contrario, non è necessario utilizzare l'attributo **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1035">Otherwise, the **EntitySet** attribute must not be used.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="cb8fc-1036">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **returnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1036">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (FunctionImport) element.</span></span> <span data-ttu-id="cb8fc-1037">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1037">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-1038">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1038">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="cb8fc-1039">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1039">Example</span></span>

<span data-ttu-id="cb8fc-1040">Nell'esempio seguente viene usato un **FunctionImport** che restituisce libri e autori.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1040">The following example uses a **FunctionImport** that returns books and publishers.</span></span> <span data-ttu-id="cb8fc-1041">Si noti che la funzione restituisce due set di risultati e pertanto sono specificati due elementi **returnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1041">Note that the function returns two result sets and therefore two **ReturnType** (FunctionImport) elements are specified.</span></span>

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a><span data-ttu-id="cb8fc-1042">Elemento RowType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1042">RowType Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-1043">Un elemento **RowType** in Conceptual Schema Definition Language (CSDL) definisce una struttura senza nome come parametro o tipo restituito per una funzione definita nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1043">A **RowType** element in conceptual schema definition language (CSDL) defines an unnamed structure as a parameter or return type for a function defined in the conceptual model.</span></span>

<span data-ttu-id="cb8fc-1044">Un elemento **RowType** può essere figlio dei seguenti elementi:</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1044">A **RowType** element can be the child of the following elements:</span></span>

-   <span data-ttu-id="cb8fc-1045">CollectionType</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1045">CollectionType</span></span>
-   <span data-ttu-id="cb8fc-1046">Parametro</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1046">Parameter</span></span>
-   <span data-ttu-id="cb8fc-1047">ReturnType (funzione)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1047">ReturnType (Function)</span></span>

<span data-ttu-id="cb8fc-1048">Un elemento **RowType** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1048">A **RowType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cb8fc-1049">Property (uno o più)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1049">Property (one or more)</span></span>
-   <span data-ttu-id="cb8fc-1050">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1050">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-1051">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1051">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-1052">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **RowType** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1052">Any number of annotation attributes (custom XML attributes) may be applied to the **RowType** element.</span></span> <span data-ttu-id="cb8fc-1053">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1053">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-1054">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1054">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="cb8fc-1055">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1055">Example</span></span>

<span data-ttu-id="cb8fc-1056">Nell'esempio seguente viene illustrata una funzione definita dal modello che utilizza un elemento **CollectionType** per specificare che la funzione restituisce una raccolta di righe, come specificato nell'elemento **RowType** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1056">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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

## <a name="schema-element-csdl"></a><span data-ttu-id="cb8fc-1057">Elemento Schema (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1057">Schema Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-1058">L'elemento **schema** è l'elemento radice di una definizione del modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1058">The **Schema** element is the root element of a conceptual model definition.</span></span> <span data-ttu-id="cb8fc-1059">Contiene definizioni per gli oggetti, le funzioni e i contenitori che costituiscono un modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1059">It contains definitions for the objects, functions, and containers that make up a conceptual model.</span></span>

<span data-ttu-id="cb8fc-1060">L'elemento **schema** può contenere zero o più degli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1060">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="cb8fc-1061">Using</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1061">Using</span></span>
-   <span data-ttu-id="cb8fc-1062">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1062">EntityContainer</span></span>
-   <span data-ttu-id="cb8fc-1063">EntityType</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1063">EntityType</span></span>
-   <span data-ttu-id="cb8fc-1064">EnumType</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1064">EnumType</span></span>
-   <span data-ttu-id="cb8fc-1065">Associazione</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1065">Association</span></span>
-   <span data-ttu-id="cb8fc-1066">ComplexType</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1066">ComplexType</span></span>
-   <span data-ttu-id="cb8fc-1067">Funzione</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1067">Function</span></span>

<span data-ttu-id="cb8fc-1068">Un elemento **dello schema** può contenere zero o un elemento Annotation.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1068">A **Schema** element may contain zero or one Annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="cb8fc-1069">L'elemento **Function** e gli elementi Annotation sono consentiti solo in CSDL V2 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1069">The **Function** element and annotation elements are only allowed in CSDL v2 and later.</span></span>

 

<span data-ttu-id="cb8fc-1070">L'elemento **schema** utilizza l'attributo **namespace** per definire lo spazio dei nomi per il tipo di entità, il tipo complesso e gli oggetti di associazione in un modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1070">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type, complex type, and association objects in a conceptual model.</span></span> <span data-ttu-id="cb8fc-1071">All'interno di uno spazio dei nomi due oggetti non possono avere lo stesso nome.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1071">Within a namespace, no two objects can have the same name.</span></span> <span data-ttu-id="cb8fc-1072">Gli spazi dei nomi possono estendersi a più elementi **dello schema** e a più file con estensione csdl.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1072">Namespaces can span multiple **Schema** elements and multiple .csdl files.</span></span>

<span data-ttu-id="cb8fc-1073">Uno spazio dei nomi del modello concettuale è diverso dallo spazio dei nomi XML dell'elemento **schema** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1073">A conceptual model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="cb8fc-1074">Uno spazio dei nomi del modello concettuale (definito dall'attributo **namespace** ) è un contenitore logico per tipi di entità, tipi complessi e tipi di associazione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1074">A conceptual model namespace (as defined by the **Namespace** attribute) is a logical container for entity types, complex types, and association types.</span></span> <span data-ttu-id="cb8fc-1075">Lo spazio dei nomi XML, indicato dall'attributo **xmlns** , di un elemento **schema** è lo spazio dei nomi predefinito per gli elementi figlio e gli attributi dell'elemento **schema** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1075">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="cb8fc-1076">Spazi dei nomi XML nel formato https://schemas.microsoft.com/ado/YYYY/MM/edm (dove YYYY e MM rappresentano un anno e mese rispettivamente) sono riservati a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1076">XML namespaces of the form https://schemas.microsoft.com/ado/YYYY/MM/edm (where YYYY and MM represent a year and month respectively) are reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-1077">Gli elementi e gli attributi personalizzati non possono essere in spazi dei nomi che hanno questo form.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1077">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-1078">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1078">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-1079">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **schema** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1079">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="cb8fc-1080">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1080">Attribute Name</span></span> | <span data-ttu-id="cb8fc-1081">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1081">Is Required</span></span> | <span data-ttu-id="cb8fc-1082">Value</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1082">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cb8fc-1083">**Spazio dei nomi**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1083">**Namespace**</span></span>  | <span data-ttu-id="cb8fc-1084">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1084">Yes</span></span>         | <span data-ttu-id="cb8fc-1085">Spazio dei nomi del modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1085">The namespace of the conceptual model.</span></span> <span data-ttu-id="cb8fc-1086">Il valore dell'attributo **namespace** viene utilizzato per formare il nome completo di un tipo.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1086">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="cb8fc-1087">Se, ad esempio, un elemento **EntityType** denominato *Customer* si trova nello spazio dei nomi Simple. example. Model, il nome completo di **EntityType** sarà sarà SimpleExampleModel. Customer.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1087">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace, then the fully qualified name of the **EntityType** is SimpleExampleModel.Customer.</span></span> <br/> <span data-ttu-id="cb8fc-1088">Non è possibile usare le stringhe seguenti come valore per l'attributo **namespace** : **Sistema**, **temporaneo**o **EDM**.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1088">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="cb8fc-1089">Il valore dell'attributo **namespace** non può corrispondere al valore dell'attributo **namespace** nell'elemento schema SSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1089">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the SSDL Schema element.</span></span> |
| <span data-ttu-id="cb8fc-1090">**Alias**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1090">**Alias**</span></span>      | <span data-ttu-id="cb8fc-1091">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1091">No</span></span>          | <span data-ttu-id="cb8fc-1092">Identificatore utilizzato al posto del nome dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1092">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="cb8fc-1093">Se, ad esempio, un elemento **EntityType** denominato *Customer* si trova nello spazio dei nomi Simple. example. Model e il valore dell'attributo **alias** è *Model*, sarà possibile utilizzare Model. Customer come nome completo dell'elemento **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1093">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace and the value of the **Alias** attribute is *Model*, then you can use Model.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="cb8fc-1094">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **schema** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1094">Any number of annotation attributes (custom XML attributes) may be applied to the **Schema** element.</span></span> <span data-ttu-id="cb8fc-1095">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1095">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-1096">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1096">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="cb8fc-1097">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1097">Example</span></span>

<span data-ttu-id="cb8fc-1098">Nell'esempio seguente viene illustrato un elemento **dello schema** che contiene un elemento **EntityContainer** , due elementi **EntityType** e un elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1098">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema xmlns="https://schemas.microsoft.com/ado/2009/11/edm"
      xmlns:cg="https://schemas.microsoft.com/ado/2009/11/codegeneration"
      xmlns:store="https://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
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
 

 

## <a name="typeref-element-csdl"></a><span data-ttu-id="cb8fc-1099">Elemento TypeRef (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1099">TypeRef Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-1100">L'elemento **typeref** in Conceptual Schema Definition Language (CSDL) fornisce un riferimento a un tipo denominato esistente.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1100">The **TypeRef** element in conceptual schema definition language (CSDL) provides a reference to an existing named type.</span></span> <span data-ttu-id="cb8fc-1101">L'elemento **typeref** può essere un elemento figlio dell'elemento CollectionType, che viene usato per specificare che una funzione ha una raccolta come parametro o tipo restituito.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1101">The **TypeRef** element can be a child of the CollectionType element, which is used to specify that a function has a collection as a parameter or return type.</span></span>

<span data-ttu-id="cb8fc-1102">Un elemento **typeref** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1102">A **TypeRef** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cb8fc-1103">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1103">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="cb8fc-1104">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1104">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-1105">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1105">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-1106">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **typeref** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1106">The following table describes the attributes that can be applied to the **TypeRef** element.</span></span> <span data-ttu-id="cb8fc-1107">Si noti che gli attributi **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **scale**, **Unicode**e **Collation** sono applicabili solo a **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1107">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="cb8fc-1108">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1108">Attribute Name</span></span>                                                     | <span data-ttu-id="cb8fc-1109">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1109">Is Required</span></span> | <span data-ttu-id="cb8fc-1110">Value</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1110">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cb8fc-1111">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1111">**Type**</span></span>                                                           | <span data-ttu-id="cb8fc-1112">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1112">No</span></span>          | <span data-ttu-id="cb8fc-1113">Nome del tipo a cui si fa riferimento.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1113">The name of the type being referenced.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="cb8fc-1114">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1114">**Nullable**</span></span>                                                       | <span data-ttu-id="cb8fc-1115">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1115">No</span></span>          | <span data-ttu-id="cb8fc-1116">**True** (valore predefinito) o **false** a seconda che la proprietà possa avere un valore null.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1116">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="cb8fc-1117">> In CSDL V1 una proprietà di tipo complesso deve avere `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1117">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="cb8fc-1118">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1118">**DefaultValue**</span></span>                                                   | <span data-ttu-id="cb8fc-1119">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1119">No</span></span>          | <span data-ttu-id="cb8fc-1120">Valore predefinito della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1120">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="cb8fc-1121">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1121">**MaxLength**</span></span>                                                      | <span data-ttu-id="cb8fc-1122">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1122">No</span></span>          | <span data-ttu-id="cb8fc-1123">Lunghezza massima del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1123">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="cb8fc-1124">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1124">**FixedLength**</span></span>                                                    | <span data-ttu-id="cb8fc-1125">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1125">No</span></span>          | <span data-ttu-id="cb8fc-1126">**True** o **false** a seconda che il valore della proprietà venga archiviato come stringa a lunghezza fissa.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1126">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="cb8fc-1127">**Precisione**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1127">**Precision**</span></span>                                                      | <span data-ttu-id="cb8fc-1128">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1128">No</span></span>          | <span data-ttu-id="cb8fc-1129">Precisione del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1129">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="cb8fc-1130">**Scala**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1130">**Scale**</span></span>                                                          | <span data-ttu-id="cb8fc-1131">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1131">No</span></span>          | <span data-ttu-id="cb8fc-1132">Scala del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1132">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="cb8fc-1133">**SRID**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1133">**SRID**</span></span>                                                           | <span data-ttu-id="cb8fc-1134">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1134">No</span></span>          | <span data-ttu-id="cb8fc-1135">Identificatore di riferimento del sistema spaziale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1135">Spatial System Reference Identifier.</span></span> <span data-ttu-id="cb8fc-1136">Valido solo per le proprietà dei tipi spaziali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1136">Valid only for properties of spatial types.</span></span> <span data-ttu-id="cb8fc-1137">Per ulteriori informazioni, vedere [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1137">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="cb8fc-1138">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1138">**Unicode**</span></span>                                                        | <span data-ttu-id="cb8fc-1139">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1139">No</span></span>          | <span data-ttu-id="cb8fc-1140">**True** o **false** a seconda che il valore della proprietà venga archiviato come stringa Unicode.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1140">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="cb8fc-1141">**Confronto**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1141">**Collation**</span></span>                                                      | <span data-ttu-id="cb8fc-1142">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1142">No</span></span>          | <span data-ttu-id="cb8fc-1143">Stringa che specifica la sequenza di confronto da utilizzare nell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1143">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="cb8fc-1144">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **CollectionType** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1144">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="cb8fc-1145">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1145">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-1146">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1146">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="cb8fc-1147">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1147">Example</span></span>

<span data-ttu-id="cb8fc-1148">Nell'esempio seguente viene illustrata una funzione definita dal modello che utilizza l'elemento **typeref** (come figlio di un elemento **CollectionType** ) per specificare che la funzione accetta una raccolta di tipi di entità **Department** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1148">The following example shows a model-defined function that uses the **TypeRef** element (as a child of a **CollectionType** element) to specify that the function accepts a collection of **Department** entity types.</span></span>

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
 

 

## <a name="using-element-csdl"></a><span data-ttu-id="cb8fc-1149">Elemento Using (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1149">Using Element (CSDL)</span></span>

<span data-ttu-id="cb8fc-1150">L'elemento **using** in Conceptual Schema Definition Language (CSDL) importa il contenuto di un modello concettuale presente in uno spazio dei nomi diverso.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1150">The **Using** element in conceptual schema definition language (CSDL) imports the contents of a conceptual model that exists in a different namespace.</span></span> <span data-ttu-id="cb8fc-1151">Impostando il valore dell'attributo **namespace** , è possibile fare riferimento a tipi di entità, tipi complessi e tipi di associazione definiti in un altro modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1151">By setting the value of the **Namespace** attribute, you can refer to entity types, complex types, and association types that are defined in another conceptual model.</span></span> <span data-ttu-id="cb8fc-1152">Più di un elemento **using** può essere figlio di un elemento **dello schema** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1152">More than one **Using** element can be a child of a **Schema** element.</span></span>

> [!NOTE]
> <span data-ttu-id="cb8fc-1153">L'elemento **using** in CSDL non funziona esattamente come un'istruzione **using** in un linguaggio di programmazione.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1153">The **Using** element in CSDL does not function exactly like a **using** statement in a programming language.</span></span> <span data-ttu-id="cb8fc-1154">Importando uno spazio dei nomi con un'istruzione **using** in un linguaggio di programmazione, non si influiscono sugli oggetti nello spazio dei nomi originale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1154">By importing a namespace with a **using** statement in a programming language, you do not affect objects in the original namespace.</span></span> <span data-ttu-id="cb8fc-1155">In CSDL, uno spazio dei nomi importato può contenere un tipo di entità derivato da un tipo di entità nello spazio dei nomi originale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1155">In CSDL, an imported namespace can contain an entity type that is derived from an entity type in the original namespace.</span></span> <span data-ttu-id="cb8fc-1156">Ciò può influire sui set di entità dichiarati nello spazio dei nomi originale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1156">This can affect entity sets declared in the original namespace.</span></span>

 

<span data-ttu-id="cb8fc-1157">L'elemento **using** può includere i seguenti elementi figlio:</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1157">The **Using** element can have the following child elements:</span></span>

-   <span data-ttu-id="cb8fc-1158">Documentazione (zero o un elemento consentito)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1158">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="cb8fc-1159">Elementi Annotation (zero o più elementi consentiti)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1159">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cb8fc-1160">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1160">Applicable Attributes</span></span>

<span data-ttu-id="cb8fc-1161">La tabella seguente descrive gli attributi che possono essere applicati all'elemento **using** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1161">The table below describes the attributes can be applied to the **Using** element.</span></span>

| <span data-ttu-id="cb8fc-1162">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1162">Attribute Name</span></span> | <span data-ttu-id="cb8fc-1163">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1163">Is Required</span></span> | <span data-ttu-id="cb8fc-1164">Value</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1164">Value</span></span>                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cb8fc-1165">**Spazio dei nomi**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1165">**Namespace**</span></span>  | <span data-ttu-id="cb8fc-1166">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1166">Yes</span></span>         | <span data-ttu-id="cb8fc-1167">Nome dello spazio dei nomi importato.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1167">The name of the imported namespace.</span></span>                                                                                                                                                |
| <span data-ttu-id="cb8fc-1168">**Alias**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1168">**Alias**</span></span>      | <span data-ttu-id="cb8fc-1169">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1169">Yes</span></span>         | <span data-ttu-id="cb8fc-1170">Identificatore utilizzato al posto del nome dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1170">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="cb8fc-1171">Anche se questo attributo è obbligatorio, non è necessario utilizzarlo al posto del nome dello spazio dei nomi per qualificare nomi di oggetti.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1171">Although this attribute is required, it is not required that it be used in place of the namespace name to qualify object names.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="cb8fc-1172">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **using** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1172">Any number of annotation attributes (custom XML attributes) may be applied to the **Using** element.</span></span> <span data-ttu-id="cb8fc-1173">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1173">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cb8fc-1174">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1174">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="cb8fc-1175">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1175">Example</span></span>

<span data-ttu-id="cb8fc-1176">Nell'esempio seguente viene illustrato l'elemento **using** usato per importare uno spazio dei nomi definito altrove.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1176">The following example demonstrates the **Using** element being used to import a namespace that is defined elsewhere.</span></span> <span data-ttu-id="cb8fc-1177">Si noti che lo spazio dei nomi per l'elemento **dello schema** visualizzato è `BooksModel`.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1177">Note that the namespace for the **Schema** element shown is `BooksModel`.</span></span> <span data-ttu-id="cb8fc-1178">La proprietà `Address` sull'**EntityType** `Publisher` è un tipo complesso definito nello spazio dei nomi `ExtendedBooksModel` (importato con l'elemento **using** ).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1178">The `Address` property on the `Publisher`**EntityType** is a complex type that is defined in the `ExtendedBooksModel` namespace (imported with the **Using** element).</span></span>

``` xml
 <Schema xmlns="https://schemas.microsoft.com/ado/2009/11/edm"
           xmlns:cg="https://schemas.microsoft.com/ado/2009/11/codegeneration"
           xmlns:store="https://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
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
 

 

## <a name="annotation-attributes-csdl"></a><span data-ttu-id="cb8fc-1179">Attributi di annotazione (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1179">Annotation Attributes (CSDL)</span></span>

<span data-ttu-id="cb8fc-1180">Gli attributi di annotazione in Conceptual Schema Definition Language (CSDL) sono attributi XML personalizzati nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1180">Annotation attributes in conceptual schema definition language (CSDL) are custom XML attributes in the conceptual model.</span></span> <span data-ttu-id="cb8fc-1181">Oltre ad avere una struttura XML valida, per gli attributi di annotazione hanno le caratteristiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1181">In addition to having valid XML structure, the following must be true of annotation attributes:</span></span>

-   <span data-ttu-id="cb8fc-1182">Gli attributi di annotazione non devono trovarsi in spazi dei nomi XML riservati a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1182">Annotation attributes must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="cb8fc-1183">È possibile applicare più attributi di annotazione a un determinato elemento CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1183">More than one annotation attribute may be applied to a given CSDL element.</span></span>
-   <span data-ttu-id="cb8fc-1184">I nomi completi di due attributi di annotazione non devono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1184">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="cb8fc-1185">Gli attributi di annotazione possono essere utilizzati per fornire metadati aggiuntivi sugli elementi in un modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1185">Annotation attributes can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="cb8fc-1186">È possibile accedere ai metadati contenuti negli elementi Annotation in fase di esecuzione usando le classi nello spazio dei nomi System. Data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1186">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="cb8fc-1187">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1187">Example</span></span>

<span data-ttu-id="cb8fc-1188">Nell'esempio seguente viene illustrato un elemento **EntityType** con un attributo Annotation (**CustomAttribute**).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1188">The following example shows an **EntityType** element with an annotation attribute (**CustomAttribute**).</span></span> <span data-ttu-id="cb8fc-1189">Nell'esempio viene inoltre mostrato un elemento di annotazione applicato all'elemento del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1189">The example also shows an annotation element applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="https://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="https://schemas.microsoft.com/ado/2009/11/edm">
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
 

<span data-ttu-id="cb8fc-1190">Il codice seguente recupera i metadati dell'attributo di annotazione e li scrive nella console:</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1190">The following code retrieves the metadata in the annotation attribute and writes it to the console:</span></span>

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
 

<span data-ttu-id="cb8fc-1191">Nel codice riportato in precedenza si presuppone che il file `School.csdl` si trovi nella directory di output del progetto e che al progetto siano state aggiunte le istruzioni `Imports` e `Using` seguenti:</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1191">The code above assumes that the `School.csdl` file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a><span data-ttu-id="cb8fc-1192">Elementi di annotazione (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1192">Annotation Elements (CSDL)</span></span>

<span data-ttu-id="cb8fc-1193">Gli elementi Annotation in Conceptual Schema Definition Language (CSDL) sono elementi XML personalizzati nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1193">Annotation elements in conceptual schema definition language (CSDL) are custom XML elements in the conceptual model.</span></span> <span data-ttu-id="cb8fc-1194">Oltre ad avere una struttura XML valida, gli elementi Annotation hanno le caratteristiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1194">In addition to having valid XML structure, the following must be true of annotation elements:</span></span>

-   <span data-ttu-id="cb8fc-1195">Gli elementi Annotation non devono trovarsi in spazi dei nomi XML riservati a CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1195">Annotation elements must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="cb8fc-1196">Più elementi Annotation possono essere figli di un dato elemento CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1196">More than one annotation element may be a child of a given CSDL element.</span></span>
-   <span data-ttu-id="cb8fc-1197">I nomi completi di due elementi Annotation non devono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1197">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="cb8fc-1198">Gli elementi Annotation devono apparire dopo tutti gli altri elementi figlio di un dato elemento CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1198">Annotation elements must appear after all other child elements of a given CSDL element.</span></span>

<span data-ttu-id="cb8fc-1199">Gli elementi Annotation possono essere utilizzati per fornire metadati aggiuntivi sugli elementi in un modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1199">Annotation elements can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="cb8fc-1200">A partire da .NET Framework versione 4, è possibile accedere ai metadati contenuti negli elementi di annotazione in fase di esecuzione usando le classi nello spazio dei nomi System. Data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1200">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="cb8fc-1201">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1201">Example</span></span>

<span data-ttu-id="cb8fc-1202">Nell'esempio seguente viene illustrato un elemento **EntityType** con un elemento Annotation (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1202">The following example shows an **EntityType** element with an annotation element (**CustomElement**).</span></span> <span data-ttu-id="cb8fc-1203">Nell'esempio viene inoltre mostrato un attributo di annotazione applicato all'elemento del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1203">The example also show an annotation attribute applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="https://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="https://schemas.microsoft.com/ado/2009/11/edm">
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
 

<span data-ttu-id="cb8fc-1204">Il codice seguente recupera i metadati dell'elemento Annotation e li scrive nella console:</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1204">The following code retrieves the metadata in the annotation element and writes it to the console:</span></span>

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
 

<span data-ttu-id="cb8fc-1205">Nel codice riportato in precedenza si presuppone che il file School.csdl si trovi nella directory di output del progetto e che al progetto siano state aggiunte le istruzioni `Imports` e `Using` seguenti:</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1205">The code above assumes that the School.csdl file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a><span data-ttu-id="cb8fc-1206">Tipi del modello concettuale (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1206">Conceptual Model Types (CSDL)</span></span>

<span data-ttu-id="cb8fc-1207">CSDL (Conceptual Schema Definition Language) supporta un set di tipi di dati primitivi astratti, denominati **EDMSimpleTypes**, che definiscono le proprietà in un modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1207">Conceptual schema definition language (CSDL) supports a set of abstract primitive data types, called **EDMSimpleTypes**, that define properties in a conceptual model.</span></span> <span data-ttu-id="cb8fc-1208">**EDMSimpleTypes** sono proxy per i tipi di dati primitivi supportati nell'ambiente di archiviazione o host.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1208">**EDMSimpleTypes** are proxies for primitive data types that are supported in the storage or hosting environment.</span></span>

<span data-ttu-id="cb8fc-1209">Nella tabella seguente vengono elencati i tipi di dati primitivi supportati da CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1209">The table below lists the primitive data types that are supported by CSDL.</span></span> <span data-ttu-id="cb8fc-1210">Nella tabella sono inoltre elencati i facet che è possibile applicare a ogni **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1210">The table also lists the facets that can be applied to each **EDMSimpleType**.</span></span>

| <span data-ttu-id="cb8fc-1211">EDMSimpleType</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1211">EDMSimpleType</span></span>                    | <span data-ttu-id="cb8fc-1212">Descrizione</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1212">Description</span></span>                                                | <span data-ttu-id="cb8fc-1213">Facet applicabili</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1213">Applicable Facets</span></span>                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| <span data-ttu-id="cb8fc-1214">**EDM. Binary**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1214">**Edm.Binary**</span></span>                   | <span data-ttu-id="cb8fc-1215">Contiene dati binari.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1215">Contains binary data.</span></span>                                      | <span data-ttu-id="cb8fc-1216">MaxLength, FixedLength, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1216">MaxLength, FixedLength, Nullable, Default</span></span>                                |
| <span data-ttu-id="cb8fc-1217">**EDM. Boolean**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1217">**Edm.Boolean**</span></span>                  | <span data-ttu-id="cb8fc-1218">Contiene il valore **true** o **false**.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1218">Contains the value **true** or **false**.</span></span>                  | <span data-ttu-id="cb8fc-1219">Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1219">Nullable, Default</span></span>                                                        |
| <span data-ttu-id="cb8fc-1220">**EDM. byte**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1220">**Edm.Byte**</span></span>                     | <span data-ttu-id="cb8fc-1221">Contiene un Unsigned Integer a 8 bit.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1221">Contains an unsigned 8-bit integer value.</span></span>                  | <span data-ttu-id="cb8fc-1222">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1222">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="cb8fc-1223">**EDM. DateTime**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1223">**Edm.DateTime**</span></span>                 | <span data-ttu-id="cb8fc-1224">Rappresenta una data e un'ora.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1224">Represents a date and time.</span></span>                                | <span data-ttu-id="cb8fc-1225">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1225">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="cb8fc-1226">**EDM. DateTimeOffset**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1226">**Edm.DateTimeOffset**</span></span>           | <span data-ttu-id="cb8fc-1227">Contiene una data e un'ora come offset in minuti rispetto all'ora GMT.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1227">Contains a date and time as an offset in minutes from GMT.</span></span> | <span data-ttu-id="cb8fc-1228">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1228">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="cb8fc-1229">**EDM. Decimal**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1229">**Edm.Decimal**</span></span>                  | <span data-ttu-id="cb8fc-1230">Contiene un valore numerico con scala e precisione fisse.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1230">Contains a numeric value with fixed precision and scale.</span></span>   | <span data-ttu-id="cb8fc-1231">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1231">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="cb8fc-1232">**EDM. Double**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1232">**Edm.Double**</span></span>                   | <span data-ttu-id="cb8fc-1233">Contiene un numero a virgola mobile con precisione di 15 cifre</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1233">Contains a floating point number with 15-digit precision</span></span>   | <span data-ttu-id="cb8fc-1234">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1234">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="cb8fc-1235">**EDM. float**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1235">**Edm.Float**</span></span>                    | <span data-ttu-id="cb8fc-1236">Contiene un numero a virgola mobile con precisione a 7 cifre.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1236">Contains a floating point number with 7-digit precision.</span></span>   | <span data-ttu-id="cb8fc-1237">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1237">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="cb8fc-1238">**EDM. Guid**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1238">**Edm.Guid**</span></span>                     | <span data-ttu-id="cb8fc-1239">Contiene un identificatore univoco a 16 byte.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1239">Contains a 16-byte unique identifier.</span></span>                      | <span data-ttu-id="cb8fc-1240">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1240">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="cb8fc-1241">**EDM. Int16**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1241">**Edm.Int16**</span></span>                    | <span data-ttu-id="cb8fc-1242">Contiene un Signed Integer a 16 bit.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1242">Contains a signed 16-bit integer value.</span></span>                    | <span data-ttu-id="cb8fc-1243">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1243">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="cb8fc-1244">**EDM. Int32**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1244">**Edm.Int32**</span></span>                    | <span data-ttu-id="cb8fc-1245">Contiene un Signed Integer a 32 bit.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1245">Contains a signed 32-bit integer value.</span></span>                    | <span data-ttu-id="cb8fc-1246">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1246">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="cb8fc-1247">**EDM. Int64**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1247">**Edm.Int64**</span></span>                    | <span data-ttu-id="cb8fc-1248">Contiene un Signed Integer a 64 bit.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1248">Contains a signed 64-bit integer value.</span></span>                    | <span data-ttu-id="cb8fc-1249">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1249">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="cb8fc-1250">**EDM. SByte**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1250">**Edm.SByte**</span></span>                    | <span data-ttu-id="cb8fc-1251">Contiene un Signed Integer a 8 bit.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1251">Contains a signed 8-bit integer value.</span></span>                     | <span data-ttu-id="cb8fc-1252">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1252">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="cb8fc-1253">**EDM. String**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1253">**Edm.String**</span></span>                   | <span data-ttu-id="cb8fc-1254">Contiene dati di tipo carattere.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1254">Contains character data.</span></span>                                   | <span data-ttu-id="cb8fc-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span></span> |
| <span data-ttu-id="cb8fc-1256">**EDM. ora**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1256">**Edm.Time**</span></span>                     | <span data-ttu-id="cb8fc-1257">Contiene un'ora del giorno.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1257">Contains a time of day.</span></span>                                    | <span data-ttu-id="cb8fc-1258">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1258">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="cb8fc-1259">**EDM. Geography**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1259">**Edm.Geography**</span></span>                |                                                            | <span data-ttu-id="cb8fc-1260">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1260">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="cb8fc-1261">**EDM. GeographyPoint**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1261">**Edm.GeographyPoint**</span></span>           |                                                            | <span data-ttu-id="cb8fc-1262">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1262">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="cb8fc-1263">**EDM. GeographyLineString**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1263">**Edm.GeographyLineString**</span></span>      |                                                            | <span data-ttu-id="cb8fc-1264">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1264">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="cb8fc-1265">**EDM. GeographyPolygon**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1265">**Edm.GeographyPolygon**</span></span>         |                                                            | <span data-ttu-id="cb8fc-1266">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1266">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="cb8fc-1267">**EDM. GeographyMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1267">**Edm.GeographyMultiPoint**</span></span>      |                                                            | <span data-ttu-id="cb8fc-1268">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1268">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="cb8fc-1269">**EDM. GeographyMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1269">**Edm.GeographyMultiLineString**</span></span> |                                                            | <span data-ttu-id="cb8fc-1270">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1270">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="cb8fc-1271">**EDM. GeographyMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1271">**Edm.GeographyMultiPolygon**</span></span>    |                                                            | <span data-ttu-id="cb8fc-1272">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1272">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="cb8fc-1273">**EDM. GeographyCollection**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1273">**Edm.GeographyCollection**</span></span>      |                                                            | <span data-ttu-id="cb8fc-1274">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1274">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="cb8fc-1275">**EDM. Geometry**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1275">**Edm.Geometry**</span></span>                 |                                                            | <span data-ttu-id="cb8fc-1276">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1276">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="cb8fc-1277">**EDM. GeometryPoint**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1277">**Edm.GeometryPoint**</span></span>            |                                                            | <span data-ttu-id="cb8fc-1278">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1278">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="cb8fc-1279">**EDM. GeometryLineString**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1279">**Edm.GeometryLineString**</span></span>       |                                                            | <span data-ttu-id="cb8fc-1280">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1280">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="cb8fc-1281">**EDM. GeometryPolygon**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1281">**Edm.GeometryPolygon**</span></span>          |                                                            | <span data-ttu-id="cb8fc-1282">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1282">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="cb8fc-1283">**EDM. GeometryMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1283">**Edm.GeometryMultiPoint**</span></span>       |                                                            | <span data-ttu-id="cb8fc-1284">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1284">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="cb8fc-1285">**EDM. GeometryMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1285">**Edm.GeometryMultiLineString**</span></span>  |                                                            | <span data-ttu-id="cb8fc-1286">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1286">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="cb8fc-1287">**EDM. GeometryMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1287">**Edm.GeometryMultiPolygon**</span></span>     |                                                            | <span data-ttu-id="cb8fc-1288">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1288">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="cb8fc-1289">**EDM. GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1289">**Edm.GeometryCollection**</span></span>       |                                                            | <span data-ttu-id="cb8fc-1290">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1290">Nullable, Default, SRID</span></span>                                                  |

## <a name="facets-csdl"></a><span data-ttu-id="cb8fc-1291">Facet (CSDL)</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1291">Facets (CSDL)</span></span>

<span data-ttu-id="cb8fc-1292">I facet in Conceptual Schema Definition Language (CSDL) rappresentano vincoli sulle proprietà di tipi di entità e di tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1292">Facets in conceptual schema definition language (CSDL) represent constraints on properties of entity types and complex types.</span></span> <span data-ttu-id="cb8fc-1293">I facet appaiono come attributi XML negli elementi CSDL seguenti:</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1293">Facets appear as XML attributes on the following CSDL elements:</span></span>

-   <span data-ttu-id="cb8fc-1294">Proprietà</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1294">Property</span></span>
-   <span data-ttu-id="cb8fc-1295">TypeRef</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1295">TypeRef</span></span>
-   <span data-ttu-id="cb8fc-1296">Parametro</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1296">Parameter</span></span>

<span data-ttu-id="cb8fc-1297">Nella tabella seguente vengono descritti i facet supportati in CSDL.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1297">The following table describes the facets that are supported in CSDL.</span></span> <span data-ttu-id="cb8fc-1298">Tutti i facet sono facoltativi.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1298">All facets are optional.</span></span> <span data-ttu-id="cb8fc-1299">Alcuni facet elencati di seguito vengono usati dal Entity Framework durante la generazione di un database da un modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1299">Some facets listed below are used by the Entity Framework when generating a database from a conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="cb8fc-1300">Per informazioni sui tipi di dati in un modello concettuale, vedere tipi di modelli concettuali (CSDL).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1300">For information about data types in a conceptual model, see Conceptual Model Types (CSDL).</span></span>

| <span data-ttu-id="cb8fc-1301">Facet</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1301">Facet</span></span>               | <span data-ttu-id="cb8fc-1302">Descrizione</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1302">Description</span></span>                                                                                                                                                                                                                                                   | <span data-ttu-id="cb8fc-1303">Si applica a</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1303">Applies to</span></span>                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="cb8fc-1304">Utilizzato per la generazione di database.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1304">Used for the database generation</span></span> | <span data-ttu-id="cb8fc-1305">Utilizzato dal runtime</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1305">Used by the runtime</span></span> |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| <span data-ttu-id="cb8fc-1306">**Confronto**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1306">**Collation**</span></span>       | <span data-ttu-id="cb8fc-1307">Specifica la sequenza di ordinamento da usare quando si eseguono operazioni di confronto e di ordinamento su valori della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1307">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                               | <span data-ttu-id="cb8fc-1308">**EDM. String**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1308">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="cb8fc-1309">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1309">Yes</span></span>                              | <span data-ttu-id="cb8fc-1310">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1310">No</span></span>                  |
| <span data-ttu-id="cb8fc-1311">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1311">**ConcurrencyMode**</span></span> | <span data-ttu-id="cb8fc-1312">Indica che il valore della proprietà deve essere usato per le verifiche della concorrenza ottimistica.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1312">Indicates that the value of the property should be used for optimistic concurrency checks.</span></span>                                                                                                                                                                    | <span data-ttu-id="cb8fc-1313">Tutte le proprietà di **EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1313">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="cb8fc-1314">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1314">No</span></span>                               | <span data-ttu-id="cb8fc-1315">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1315">Yes</span></span>                 |
| <span data-ttu-id="cb8fc-1316">**Default**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1316">**Default**</span></span>         | <span data-ttu-id="cb8fc-1317">Specifica il valore predefinito della proprietà se durante la creazione di istanze non viene fornito alcun valore.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1317">Specifies the default value of the property if no value is supplied upon instantiation.</span></span>                                                                                                                                                                       | <span data-ttu-id="cb8fc-1318">Tutte le proprietà di **EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1318">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="cb8fc-1319">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1319">Yes</span></span>                              | <span data-ttu-id="cb8fc-1320">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1320">Yes</span></span>                 |
| <span data-ttu-id="cb8fc-1321">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1321">**FixedLength**</span></span>     | <span data-ttu-id="cb8fc-1322">Specifica se la lunghezza del valore della proprietà può variare.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1322">Specifies whether the length of the property value can vary.</span></span>                                                                                                                                                                                                  | <span data-ttu-id="cb8fc-1323">**EDM. Binary**, **EDM. String**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1323">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="cb8fc-1324">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1324">Yes</span></span>                              | <span data-ttu-id="cb8fc-1325">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1325">No</span></span>                  |
| <span data-ttu-id="cb8fc-1326">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1326">**MaxLength**</span></span>       | <span data-ttu-id="cb8fc-1327">Specifica la lunghezza massima del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1327">Specifies the maximum length of the property value.</span></span>                                                                                                                                                                                                           | <span data-ttu-id="cb8fc-1328">**EDM. Binary**, **EDM. String**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1328">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="cb8fc-1329">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1329">Yes</span></span>                              | <span data-ttu-id="cb8fc-1330">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1330">No</span></span>                  |
| <span data-ttu-id="cb8fc-1331">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1331">**Nullable**</span></span>        | <span data-ttu-id="cb8fc-1332">Specifica se la proprietà può avere un valore **null** .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1332">Specifies whether the property can have a **null** value.</span></span>                                                                                                                                                                                                     | <span data-ttu-id="cb8fc-1333">Tutte le proprietà di **EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1333">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="cb8fc-1334">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1334">Yes</span></span>                              | <span data-ttu-id="cb8fc-1335">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1335">Yes</span></span>                 |
| <span data-ttu-id="cb8fc-1336">**Precisione**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1336">**Precision**</span></span>       | <span data-ttu-id="cb8fc-1337">Per le proprietà di tipo **Decimal**, specifica il numero di cifre che un valore della proprietà può avere.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1337">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="cb8fc-1338">Per le proprietà di tipo **Time**, **DateTime**e **DateTimeOffset**, specifica il numero di cifre per la parte frazionaria dei secondi del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1338">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the property value.</span></span> | <span data-ttu-id="cb8fc-1339">**EDM. DateTime**, **EDM. DateTimeOffset**, **EDM. Decimal**, **EDM. Time**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span></span>                                                                                                                                                                                                                                                                                                              | <span data-ttu-id="cb8fc-1340">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1340">Yes</span></span>                              | <span data-ttu-id="cb8fc-1341">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1341">No</span></span>                  |
| <span data-ttu-id="cb8fc-1342">**Scala**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1342">**Scale**</span></span>           | <span data-ttu-id="cb8fc-1343">Specifica il numero di cifre a destra del separatore decimale per il valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1343">Specifies the number of digits to the right of the decimal point for the property value.</span></span>                                                                                                                                                                      | <span data-ttu-id="cb8fc-1344">**EDM. Decimal**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1344">**Edm.Decimal**</span></span>                                                                                                                                                                                                                                                                                                                                                                      | <span data-ttu-id="cb8fc-1345">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1345">Yes</span></span>                              | <span data-ttu-id="cb8fc-1346">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1346">No</span></span>                  |
| <span data-ttu-id="cb8fc-1347">**SRID**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1347">**SRID**</span></span>            | <span data-ttu-id="cb8fc-1348">Specifica l'ID del sistema di riferimento del sistema spaziale.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1348">Specifies the Spatial System Reference System ID.</span></span> <span data-ttu-id="cb8fc-1349">Per ulteriori informazioni, vedere [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1349">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span>                                                              | <span data-ttu-id="cb8fc-1350">**EDM. Geography, Edm. GeographyPoint, Edm. GeographyLineString, Edm. GeographyPolygon, Edm. GeographyMultiPoint, Edm. GeographyMultiLineString, Edm. GeographyMultiPolygon, Edm. GeographyCollection, Edm. Geometry, Edm. GeometryPoint, EDM. GeometryLineString, Edm. GeometryPolygon, Edm. GeometryMultiPoint, Edm. GeometryMultiLineString, Edm. GeometryMultiPolygon, Edm. GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1350">**Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span></span> | <span data-ttu-id="cb8fc-1351">No</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1351">No</span></span>                               | <span data-ttu-id="cb8fc-1352">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1352">Yes</span></span>                 |
| <span data-ttu-id="cb8fc-1353">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1353">**Unicode**</span></span>         | <span data-ttu-id="cb8fc-1354">Viene indicato se il valore della proprietà viene archiviato come Unicode.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1354">Indicates whether the property value is stored as Unicode.</span></span>                                                                                                                                                                                                    | <span data-ttu-id="cb8fc-1355">**EDM. String**</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1355">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="cb8fc-1356">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1356">Yes</span></span>                              | <span data-ttu-id="cb8fc-1357">Yes</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1357">Yes</span></span>                 |

>[!NOTE]
> <span data-ttu-id="cb8fc-1358">Durante la generazione di un database da un modello concettuale, la procedura guidata Crea Database riconosceranno il valore della **StoreGeneratedPattern** dell'attributo su un **proprietà** elemento se è nel seguente spazio dei nomi: https://schemas.microsoft.com/ado/2009/02/edm/annotation .</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1358">When generating a database from a conceptual model, the Generate Database Wizard will recognize the value of the **StoreGeneratedPattern** attribute on a **Property** element if it is in the following namespace: https://schemas.microsoft.com/ado/2009/02/edm/annotation.</span></span> <span data-ttu-id="cb8fc-1359">I valori supportati per l'attributo sono **Identity** e **calcolato**.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1359">The supported values for the attribute are **Identity** and **Computed**.</span></span> <span data-ttu-id="cb8fc-1360">Il valore **Identity** produrrà una colonna di database con un valore Identity generato nel database.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1360">A value of **Identity** will produce a database column with an identity value that is generated in the database.</span></span> <span data-ttu-id="cb8fc-1361">Il valore **calcolato** produrrà una colonna con un valore calcolato nel database.</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1361">A value of **Computed** will produce a column with a value that is computed in the database.</span></span>

### <a name="example"></a><span data-ttu-id="cb8fc-1362">Esempio</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1362">Example</span></span>

<span data-ttu-id="cb8fc-1363">Nell'esempio seguente vengono mostrati i facet applicati alle proprietà di un tipo di entità:</span><span class="sxs-lookup"><span data-stu-id="cb8fc-1363">The following example shows facets applied to the properties of an entity type:</span></span>

``` xml
 <EntityType Name="Product">
   <Key>
     <PropertyRef Name="ProductId" />
   </Key>
   <Property Type="Int32"
             Name="ProductId" Nullable="false"
             a:StoreGeneratedPattern="Identity"
    xmlns:a="https://schemas.microsoft.com/ado/2009/02/edm/annotation" />
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
