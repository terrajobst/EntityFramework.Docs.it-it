---
title: Specifica di CSDL - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
caps.latest.revision: 3
ms.openlocfilehash: 0ece73a19fe7ea244905bccb728ab2a104c5179f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121208"
---
# <a name="csdl-specification"></a><span data-ttu-id="7bd01-102">Specifica CSDL</span><span class="sxs-lookup"><span data-stu-id="7bd01-102">CSDL Specification</span></span>
<span data-ttu-id="7bd01-103">Conceptual Schema Definition Language (CSDL) è un linguaggio basato su XML che descrive le entità, le relazioni e le funzioni che costituiscono un modello concettuale di un'applicazione basata sui dati.</span><span class="sxs-lookup"><span data-stu-id="7bd01-103">Conceptual schema definition language (CSDL) is an XML-based language that describes the entities, relationships, and functions that make up a conceptual model of a data-driven application.</span></span> <span data-ttu-id="7bd01-104">Questo modello concettuale può essere usato da Entity Framework o WCF Data Services.</span><span class="sxs-lookup"><span data-stu-id="7bd01-104">This conceptual model can be used by the Entity Framework or WCF Data Services.</span></span> <span data-ttu-id="7bd01-105">I metadati descritti con CSDL vengono utilizzati da Entity Framework per eseguire il mapping di entità e relazioni definite in un modello concettuale a un'origine dati.</span><span class="sxs-lookup"><span data-stu-id="7bd01-105">The metadata that is described with CSDL is used by the Entity Framework to map entities and relationships that are defined in a conceptual model to a data source.</span></span> <span data-ttu-id="7bd01-106">Per altre informazioni, vedere [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) e [specifica MSL](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="7bd01-106">For more information, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) and [MSL Specification](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span></span>

<span data-ttu-id="7bd01-107">CSDL è l'implementazione di Entity Framework di Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="7bd01-107">CSDL is the Entity Framework's implementation of the Entity Data Model.</span></span>

<span data-ttu-id="7bd01-108">In un'applicazione Entity Framework, i metadati del modello concettuale viene caricato da un file con estensione CSDL, scritto in CSDL, in un'istanza del System.Data.Metadata.Edm.EdmItemCollection e sia accessibili tramite i metodi di Classe System.Data.Metadata.Edm.MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="7bd01-108">In an Entity Framework application, conceptual model metadata is loaded from a .csdl file (written in CSDL) into an instance of the System.Data.Metadata.Edm.EdmItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="7bd01-109">Entity Framework Usa i metadati del modello concettuale per tradurre le query sul modello concettuale in comandi specifici dell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="7bd01-109">Entity Framework uses conceptual model metadata to translate queries against the conceptual model to data source-specific commands.</span></span>

<span data-ttu-id="7bd01-110">Entity Framework Designer archivia informazioni sul modello concettuale in un file con estensione edmx in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-110">The EF Designer stores conceptual model information in an .edmx file at design time.</span></span> <span data-ttu-id="7bd01-111">In fase di compilazione, Entity Framework Designer utilizza le informazioni in un file con estensione edmx per creare il file con estensione CSDL che serve da Entity Framework in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-111">At build time, the EF Designer uses information in an .edmx file to create the .csdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="7bd01-112">Le versioni di CSDL si differenziano tra loro per gli spazi dei nomi XML.</span><span class="sxs-lookup"><span data-stu-id="7bd01-112">Versions of CSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="7bd01-113">Versione CSDL</span><span class="sxs-lookup"><span data-stu-id="7bd01-113">CSDL Version</span></span> | <span data-ttu-id="7bd01-114">Namespace XML</span><span class="sxs-lookup"><span data-stu-id="7bd01-114">XML Namespace</span></span>                                |
|:-------------|:---------------------------------------------|
| <span data-ttu-id="7bd01-115">CSDL v1</span><span class="sxs-lookup"><span data-stu-id="7bd01-115">CSDL v1</span></span>      | http://schemas.microsoft.com/ado/2006/04/edm |
| <span data-ttu-id="7bd01-116">CSDL v2</span><span class="sxs-lookup"><span data-stu-id="7bd01-116">CSDL v2</span></span>      | http://schemas.microsoft.com/ado/2008/09/edm |
| <span data-ttu-id="7bd01-117">CSDL v3</span><span class="sxs-lookup"><span data-stu-id="7bd01-117">CSDL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a><span data-ttu-id="7bd01-118">Elemento Association (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-118">Association Element (CSDL)</span></span>

<span data-ttu-id="7bd01-119">Un' **Association** elemento definisce una relazione tra due tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-119">An **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="7bd01-120">Un'associazione deve specificare i tipi di entità coinvolti nella relazione e il possibile numero di tipi di entità a ogni entità finale della relazione, che è noto come molteplicità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-120">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="7bd01-121">La molteplicità di un'entità finale dell'associazione può avere un valore di uno (1), zero o uno (0..1) o molti (\*).</span><span class="sxs-lookup"><span data-stu-id="7bd01-121">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="7bd01-122">Queste informazioni vengono specificate in due elementi End figlio.</span><span class="sxs-lookup"><span data-stu-id="7bd01-122">This information is specified in two child End elements.</span></span>

<span data-ttu-id="7bd01-123">Le istanze del tipo di entità in corrispondenza di un'entità finale di un'associazione sono accessibili attraverso proprietà di navigazione o chiavi esterne se sono esposte in un tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-123">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys, if they are exposed on an entity type.</span></span>

<span data-ttu-id="7bd01-124">In un'applicazione, un'istanza di un'associazione rappresenta un'associazione specifica tra istanze di tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-124">In an application, an instance of an association represents a specific association between instances of entity types.</span></span> <span data-ttu-id="7bd01-125">Le istanze dell'associazione sono raggruppate logicamente in un set di associazioni.</span><span class="sxs-lookup"><span data-stu-id="7bd01-125">Association instances are logically grouped in an association set.</span></span>

<span data-ttu-id="7bd01-126">Un' **Association** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="7bd01-126">An **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="7bd01-127">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="7bd01-127">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="7bd01-128">End (esattamente 2 elementi)</span><span class="sxs-lookup"><span data-stu-id="7bd01-128">End (exactly 2 elements)</span></span>
-   <span data-ttu-id="7bd01-129">ReferentialConstraint (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="7bd01-129">ReferentialConstraint (zero or one element)</span></span>
-   <span data-ttu-id="7bd01-130">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="7bd01-130">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-131">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-131">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-132">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **Association** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-132">The table below describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="7bd01-133">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="7bd01-133">Attribute Name</span></span> | <span data-ttu-id="7bd01-134">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="7bd01-134">Is Required</span></span> | <span data-ttu-id="7bd01-135">Valore</span><span class="sxs-lookup"><span data-stu-id="7bd01-135">Value</span></span>                        |
|:---------------|:------------|:-----------------------------|
| <span data-ttu-id="7bd01-136">**Name**</span><span class="sxs-lookup"><span data-stu-id="7bd01-136">**Name**</span></span>       | <span data-ttu-id="7bd01-137">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-137">Yes</span></span>         | <span data-ttu-id="7bd01-138">Nome dell'associazione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-138">The name of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="7bd01-139">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **Association** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-139">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="7bd01-140">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-140">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-141">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-141">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="7bd01-142">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-142">Example</span></span>

<span data-ttu-id="7bd01-143">L'esempio seguente mostra un' **Association** elemento che definisce il **CustomerOrders** associazione quando le chiavi esterne non sono state esposte sul **cliente** e  **Ordine** i tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-143">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have not been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="7bd01-144">Il **molteplicità** i valori per ogni **finali** dell'associazione indicano che molte **ordini** può essere associato un **cliente**, ma un solo **cliente** può essere associato un **ordine**.</span><span class="sxs-lookup"><span data-stu-id="7bd01-144">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="7bd01-145">Inoltre, il **OnDelete** elemento indica che tutte le **ordini** che sono correlati a un particolare **cliente** e sono stati caricati in ObjectContext verrà eliminato Se il **cliente** viene eliminato.</span><span class="sxs-lookup"><span data-stu-id="7bd01-145">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

<span data-ttu-id="7bd01-146">L'esempio seguente mostra un' **Association** elemento che definisce il **CustomerOrders** associazione quando le chiavi esterne sono state esposte sul **cliente** e  **Ordine** i tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-146">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="7bd01-147">Con le chiavi esterne esposte, la relazione tra l'entità viene gestita con un **ReferentialConstraint** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-147">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element.</span></span> <span data-ttu-id="7bd01-148">Un elemento AssociationSetMapping corrispondente non è necessario eseguire il mapping di questa associazione all'origine dati.</span><span class="sxs-lookup"><span data-stu-id="7bd01-148">A corresponding AssociationSetMapping element is not necessary to map this association to the data source.</span></span>

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
 

 

## <a name="associationset-element-csdl"></a><span data-ttu-id="7bd01-149">Elemento AssociationSet (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-149">AssociationSet Element (CSDL)</span></span>

<span data-ttu-id="7bd01-150">Il **AssociationSet** elemento schema conceptual schema definition Language (CSDL) è un contenitore logico per le istanze dell'associazione dello stesso tipo.</span><span class="sxs-lookup"><span data-stu-id="7bd01-150">The **AssociationSet** element in conceptual schema definition language (CSDL) is a logical container for association instances of the same type.</span></span> <span data-ttu-id="7bd01-151">Un set di associazioni fornisce una definizione per il raggruppamento di istanze di associazioni in modo che possano essere mappate a un'origine dati.</span><span class="sxs-lookup"><span data-stu-id="7bd01-151">An association set provides a definition for grouping association instances so that they can be mapped to a data source.</span></span>  

<span data-ttu-id="7bd01-152">Il **AssociationSet** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="7bd01-152">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="7bd01-153">Documentazione (zero o un elemento consentito)</span><span class="sxs-lookup"><span data-stu-id="7bd01-153">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="7bd01-154">End (esattamente due elementi richiesti)</span><span class="sxs-lookup"><span data-stu-id="7bd01-154">End (exactly two elements required)</span></span>
-   <span data-ttu-id="7bd01-155">Elementi Annotation (zero o più elementi consentiti)</span><span class="sxs-lookup"><span data-stu-id="7bd01-155">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="7bd01-156">Il **Association** attributo specifica il tipo di associazione che contiene un set di associazioni.</span><span class="sxs-lookup"><span data-stu-id="7bd01-156">The **Association** attribute specifies the type of association that an association set contains.</span></span> <span data-ttu-id="7bd01-157">I set di entità che costituiscono le entità finali di un set di associazioni vengono specificati con esattamente due figlio **End** elementi.</span><span class="sxs-lookup"><span data-stu-id="7bd01-157">The entity sets that make up the ends of an association set are specified with exactly two child **End** elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-158">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-158">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-159">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **AssociationSet** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-159">The table below describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="7bd01-160">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="7bd01-160">Attribute Name</span></span>  | <span data-ttu-id="7bd01-161">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="7bd01-161">Is Required</span></span> | <span data-ttu-id="7bd01-162">Valore</span><span class="sxs-lookup"><span data-stu-id="7bd01-162">Value</span></span>                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="7bd01-163">**Name**</span><span class="sxs-lookup"><span data-stu-id="7bd01-163">**Name**</span></span>        | <span data-ttu-id="7bd01-164">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-164">Yes</span></span>         | <span data-ttu-id="7bd01-165">Nome del set di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-165">The name of the entity set.</span></span> <span data-ttu-id="7bd01-166">Il valore del **Name** non può essere identico al valore dell'attributo il **Association** attributo.</span><span class="sxs-lookup"><span data-stu-id="7bd01-166">The value of the **Name** attribute cannot be the same as the value of the **Association** attribute.</span></span>                                 |
| <span data-ttu-id="7bd01-167">**Associazione**</span><span class="sxs-lookup"><span data-stu-id="7bd01-167">**Association**</span></span> | <span data-ttu-id="7bd01-168">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-168">Yes</span></span>         | <span data-ttu-id="7bd01-169">Il nome completo dell'associazione le cui istanze sono contenute nel set di associazioni.</span><span class="sxs-lookup"><span data-stu-id="7bd01-169">The fully-qualified name of the association that the association set contains instances of.</span></span> <span data-ttu-id="7bd01-170">L'associazione deve essere nello stesso spazio dei nomi del set di associazioni.</span><span class="sxs-lookup"><span data-stu-id="7bd01-170">The association must be in the same namespace as the association set.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="7bd01-171">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **AssociationSet** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-171">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="7bd01-172">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-172">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-173">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-173">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="7bd01-174">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-174">Example</span></span>

<span data-ttu-id="7bd01-175">L'esempio seguente mostra un' **EntityContainer** elemento con due **AssociationSet** elementi:</span><span class="sxs-lookup"><span data-stu-id="7bd01-175">The following example shows an **EntityContainer** element with two **AssociationSet** elements:</span></span>

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
 

 

## <a name="collectiontype-element-csdl"></a><span data-ttu-id="7bd01-176">Elemento CollectionType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-176">CollectionType Element (CSDL)</span></span>

<span data-ttu-id="7bd01-177">Il **CollectionType** elemento schema conceptual schema definition Language (CSDL) specifica che una funzione o un parametro il tipo restituito è una raccolta.</span><span class="sxs-lookup"><span data-stu-id="7bd01-177">The **CollectionType** element in conceptual schema definition language (CSDL) specifies that a function parameter or function return type is a collection.</span></span> <span data-ttu-id="7bd01-178">Il **CollectionType** elemento può essere un figlio dell'elemento di parametro o l'elemento ReturnType (funzione).</span><span class="sxs-lookup"><span data-stu-id="7bd01-178">The **CollectionType** element can be a child of the Parameter element or the ReturnType (Function) element.</span></span> <span data-ttu-id="7bd01-179">Il tipo di raccolta può essere specificato usando il **tipo** attributo o uno degli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="7bd01-179">The type of collection can be specified by using either the **Type** attribute or one of the following child elements:</span></span>

-   <span data-ttu-id="7bd01-180">**Elemento CollectionType**</span><span class="sxs-lookup"><span data-stu-id="7bd01-180">**CollectionType**</span></span>
-   <span data-ttu-id="7bd01-181">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="7bd01-181">ReferenceType</span></span>
-   <span data-ttu-id="7bd01-182">RowType</span><span class="sxs-lookup"><span data-stu-id="7bd01-182">RowType</span></span>
-   <span data-ttu-id="7bd01-183">TypeRef</span><span class="sxs-lookup"><span data-stu-id="7bd01-183">TypeRef</span></span>

> [!NOTE]
> <span data-ttu-id="7bd01-184">Un modello non eseguirà la convalida se viene specificato il tipo di una raccolta con entrambe le **tipo** attributo e un elemento figlio.</span><span class="sxs-lookup"><span data-stu-id="7bd01-184">A model will not validate if the type of a collection is specified with both the **Type** attribute and a child element.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-185">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-185">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-186">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **CollectionType** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-186">The following table describes the attributes that can be applied to the **CollectionType** element.</span></span> <span data-ttu-id="7bd01-187">Si noti che il **DefaultValue**, **MaxLength**, **FixedLength**, **precisione**, **scalabilità**,  **Unicode**, e **regole di confronto** gli attributi sono applicabili solo alle raccolte di **semplici EDM**.</span><span class="sxs-lookup"><span data-stu-id="7bd01-187">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to collections of **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="7bd01-188">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="7bd01-188">Attribute Name</span></span>                                                          | <span data-ttu-id="7bd01-189">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="7bd01-189">Is Required</span></span> | <span data-ttu-id="7bd01-190">Valore</span><span class="sxs-lookup"><span data-stu-id="7bd01-190">Value</span></span>                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="7bd01-191">**Type**</span><span class="sxs-lookup"><span data-stu-id="7bd01-191">**Type**</span></span>                                                                | <span data-ttu-id="7bd01-192">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-192">No</span></span>          | <span data-ttu-id="7bd01-193">Tipo della raccolta.</span><span class="sxs-lookup"><span data-stu-id="7bd01-193">The type of the collection.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="7bd01-194">**Ammette valori null**</span><span class="sxs-lookup"><span data-stu-id="7bd01-194">**Nullable**</span></span>                                                            | <span data-ttu-id="7bd01-195">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-195">No</span></span>          | <span data-ttu-id="7bd01-196">**True** (valore predefinito) o **False** a seconda del fatto che la proprietà può avere un valore null.</span><span class="sxs-lookup"><span data-stu-id="7bd01-196">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                 |
| <span data-ttu-id="7bd01-197">> Nella versione 1 di CSDL, una proprietà di tipo complesso deve avere `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="7bd01-197">> In the CSDL v1, a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                  |
| <span data-ttu-id="7bd01-198">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="7bd01-198">**DefaultValue**</span></span>                                                        | <span data-ttu-id="7bd01-199">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-199">No</span></span>          | <span data-ttu-id="7bd01-200">Valore predefinito della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-200">The default value of the property.</span></span>                                                                                                                                                                                               |
| <span data-ttu-id="7bd01-201">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="7bd01-201">**MaxLength**</span></span>                                                           | <span data-ttu-id="7bd01-202">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-202">No</span></span>          | <span data-ttu-id="7bd01-203">Lunghezza massima del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-203">The maximum length of the property value.</span></span>                                                                                                                                                                                        |
| <span data-ttu-id="7bd01-204">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="7bd01-204">**FixedLength**</span></span>                                                         | <span data-ttu-id="7bd01-205">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-205">No</span></span>          | <span data-ttu-id="7bd01-206">**True** oppure **False** a seconda se il valore della proprietà essere archiviato come una stringa di lunghezza fissa.</span><span class="sxs-lookup"><span data-stu-id="7bd01-206">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                           |
| <span data-ttu-id="7bd01-207">**Precisione**</span><span class="sxs-lookup"><span data-stu-id="7bd01-207">**Precision**</span></span>                                                           | <span data-ttu-id="7bd01-208">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-208">No</span></span>          | <span data-ttu-id="7bd01-209">Precisione del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-209">The precision of the property value.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="7bd01-210">**Scala**</span><span class="sxs-lookup"><span data-stu-id="7bd01-210">**Scale**</span></span>                                                               | <span data-ttu-id="7bd01-211">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-211">No</span></span>          | <span data-ttu-id="7bd01-212">Scala del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-212">The scale of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="7bd01-213">**SRID**</span><span class="sxs-lookup"><span data-stu-id="7bd01-213">**SRID**</span></span>                                                                | <span data-ttu-id="7bd01-214">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-214">No</span></span>          | <span data-ttu-id="7bd01-215">Identificatore di riferimento spaziale del sistema.</span><span class="sxs-lookup"><span data-stu-id="7bd01-215">Spatial System Reference Identifier.</span></span> <span data-ttu-id="7bd01-216">Valido solo per le proprietà di tipi spaziali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-216">Valid only for properties of spatial types.</span></span>   <span data-ttu-id="7bd01-217">Per altre informazioni, vedere [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span><span class="sxs-lookup"><span data-stu-id="7bd01-217">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span></span> |
| <span data-ttu-id="7bd01-218">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="7bd01-218">**Unicode**</span></span>                                                             | <span data-ttu-id="7bd01-219">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-219">No</span></span>          | <span data-ttu-id="7bd01-220">**True** oppure **False** a seconda se il valore della proprietà essere archiviato come stringa Unicode.</span><span class="sxs-lookup"><span data-stu-id="7bd01-220">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                                |
| <span data-ttu-id="7bd01-221">**regole di confronto**</span><span class="sxs-lookup"><span data-stu-id="7bd01-221">**Collation**</span></span>                                                           | <span data-ttu-id="7bd01-222">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-222">No</span></span>          | <span data-ttu-id="7bd01-223">Stringa che specifica la sequenza di confronto da utilizzare nell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="7bd01-223">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                    |

 

> [!NOTE]
> <span data-ttu-id="7bd01-224">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **CollectionType** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-224">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="7bd01-225">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-226">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="7bd01-227">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-227">Example</span></span>

<span data-ttu-id="7bd01-228">L'esempio seguente illustra una funzione definita dal modello che utilizza un **CollectionType** elemento per specificare che la funzione restituisce una raccolta di **persona** i tipi di entità (come specificato con il **ElementType** attributo).</span><span class="sxs-lookup"><span data-stu-id="7bd01-228">The following example shows a model-defined function that that uses a **CollectionType** element to specify that the function returns a collection of **Person** entity types (as specified with the **ElementType** attribute).</span></span>

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
 

<span data-ttu-id="7bd01-229">L'esempio seguente illustra una funzione definita dal modello che usa un' **CollectionType** elemento per specificare che la funzione restituisce una raccolta di righe (come specificato nella **RowType** elemento).</span><span class="sxs-lookup"><span data-stu-id="7bd01-229">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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
 

<span data-ttu-id="7bd01-230">L'esempio seguente illustra una funzione definita dal modello che utilizza il **CollectionType** elemento per specificare che la funzione accetta come parametro una raccolta di **reparto** i tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-230">The following example shows a model-defined function that uses the **CollectionType** element to specify that the function accepts as a parameter a collection of **Department** entity types.</span></span>

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
 

 

## <a name="complextype-element-csdl"></a><span data-ttu-id="7bd01-231">Elemento ComplexType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-231">ComplexType Element (CSDL)</span></span>

<span data-ttu-id="7bd01-232">Oggetto **ComplexType** elemento definisce una struttura di dati composta **EdmSimpleType** proprietà o altri tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="7bd01-232">A **ComplexType** element defines a data structure composed of **EdmSimpleType** properties or other complex types.</span></span>  <span data-ttu-id="7bd01-233">Un tipo complesso può essere una proprietà di un tipo di entità o un altro tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="7bd01-233">A complex type can be a property of an entity type or another complex type.</span></span> <span data-ttu-id="7bd01-234">Un tipo complesso è simile a un tipo di entità in quanto il tipo complesso definisce i dati.</span><span class="sxs-lookup"><span data-stu-id="7bd01-234">A complex type is similar to an entity type in that a complex type defines data.</span></span> <span data-ttu-id="7bd01-235">Tuttavia esistono alcune differenze importanti tra i tipi complessi e i tipi di entità:</span><span class="sxs-lookup"><span data-stu-id="7bd01-235">However, there are some key differences between complex types and entity types:</span></span>

-   <span data-ttu-id="7bd01-236">I tipi complessi non dispongono di identità o chiavi e pertanto non possono esistere indipendentemente.</span><span class="sxs-lookup"><span data-stu-id="7bd01-236">Complex types do not have identities (or keys) and therefore cannot exist independently.</span></span> <span data-ttu-id="7bd01-237">I tipi complessi possono esistere solo come proprietà di tipi di entità o gli altri tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="7bd01-237">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="7bd01-238">I tipi complessi non possono partecipare nelle associazioni.</span><span class="sxs-lookup"><span data-stu-id="7bd01-238">Complex types cannot participate in associations.</span></span> <span data-ttu-id="7bd01-239">Né finale di un'associazione può essere un tipo complesso, e pertanto non è possibile definire le proprietà di navigazione per i tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="7bd01-239">Neither end of an association can be a complex type, and therefore navigation properties cannot be defined for complex types.</span></span>
-   <span data-ttu-id="7bd01-240">Una proprietà di tipo complesso non può avere un valore null, sebbene ogni proprietà scalare di un tipo complesso possa essere impostata su Null.</span><span class="sxs-lookup"><span data-stu-id="7bd01-240">A complex type property cannot have a null value, though the scalar properties of a complex type may each be set to null.</span></span>

<span data-ttu-id="7bd01-241">Oggetto **ComplexType** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="7bd01-241">A **ComplexType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="7bd01-242">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="7bd01-242">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="7bd01-243">Proprietà (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="7bd01-243">Property (zero or more elements)</span></span>
-   <span data-ttu-id="7bd01-244">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="7bd01-244">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="7bd01-245">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **ComplexType** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-245">The table below describes the attributes that can be applied to the **ComplexType** element.</span></span>

| <span data-ttu-id="7bd01-246">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="7bd01-246">Attribute Name</span></span>                                                                                                 | <span data-ttu-id="7bd01-247">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="7bd01-247">Is Required</span></span> | <span data-ttu-id="7bd01-248">Valore</span><span class="sxs-lookup"><span data-stu-id="7bd01-248">Value</span></span>                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="7bd01-249">nome</span><span class="sxs-lookup"><span data-stu-id="7bd01-249">Name</span></span>                                                                                                           | <span data-ttu-id="7bd01-250">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-250">Yes</span></span>         | <span data-ttu-id="7bd01-251">Nome del tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="7bd01-251">The name of the complex type.</span></span> <span data-ttu-id="7bd01-252">Il nome di un tipo complesso non può essere uguale a quello di un altro tipo complesso, di un tipo di entità o di un'associazione che si trova entro l'ambito del modello.</span><span class="sxs-lookup"><span data-stu-id="7bd01-252">The name of a complex type cannot be the same as the name of another complex type, entity type, or association that is within the scope of the model.</span></span> |
| <span data-ttu-id="7bd01-253">BaseType</span><span class="sxs-lookup"><span data-stu-id="7bd01-253">BaseType</span></span>                                                                                                       | <span data-ttu-id="7bd01-254">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-254">No</span></span>          | <span data-ttu-id="7bd01-255">Nome di un altro tipo complesso che è il tipo di base del tipo complesso definito.</span><span class="sxs-lookup"><span data-stu-id="7bd01-255">The name of another complex type that is the base type of the complex type that is being defined.</span></span> <br/> [!NOTE]                                                                     |
| <span data-ttu-id="7bd01-256">> Questo attributo non è applicabile nella versione 1 CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-256">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="7bd01-257">L'ereditarietà per i tipi complessi non è supportata in quella versione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-257">Inheritance for complex types is not supported in that version.</span></span> |             |                                                                                                                                                                                     |
| <span data-ttu-id="7bd01-258">Classe astratta</span><span class="sxs-lookup"><span data-stu-id="7bd01-258">Abstract</span></span>                                                                                                       | <span data-ttu-id="7bd01-259">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-259">No</span></span>          | <span data-ttu-id="7bd01-260">**True** oppure **False** (valore predefinito) a seconda se il tipo complesso è un tipo astratto.</span><span class="sxs-lookup"><span data-stu-id="7bd01-260">**True** or **False** (the default value) depending on whether the complex type is an abstract type.</span></span> <br/> [!NOTE]                                                                  |
| <span data-ttu-id="7bd01-261">> Questo attributo non è applicabile nella versione 1 CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-261">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="7bd01-262">I tipi complessi in quella versione non possono essere tipi astratti.</span><span class="sxs-lookup"><span data-stu-id="7bd01-262">Complex types in that version cannot be abstract types.</span></span>         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="7bd01-263">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **ComplexType** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-263">Any number of annotation attributes (custom XML attributes) may be applied to the **ComplexType** element.</span></span> <span data-ttu-id="7bd01-264">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-264">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-265">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-265">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="7bd01-266">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-266">Example</span></span>

<span data-ttu-id="7bd01-267">L'esempio seguente mostra un tipo complesso, **indirizzi**, con la **EdmSimpleType** delle proprietà **StreetAddress**, **Città**,  **StateOrProvince**, **paese**, e **PostalCode**.</span><span class="sxs-lookup"><span data-stu-id="7bd01-267">The following example shows a complex type, **Address**, with the **EdmSimpleType** properties **StreetAddress**, **City**, **StateOrProvince**, **Country**, and **PostalCode**.</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

<span data-ttu-id="7bd01-268">Per definire il tipo complesso **indirizzo** (vedere sopra) come una proprietà di un tipo di entità, è necessario dichiarare il tipo della proprietà nella definizione del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-268">To define the complex type **Address** (above) as a property of an entity type, you must declare the property type in the entity type definition.</span></span> <span data-ttu-id="7bd01-269">L'esempio seguente mostra le **indirizzi** proprietà come un tipo complesso di un tipo di entità (**server di pubblicazione**):</span><span class="sxs-lookup"><span data-stu-id="7bd01-269">The following example shows the **Address** property as a complex type on an entity type (**Publisher**):</span></span>

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
 

 

## <a name="definingexpression-element-csdl"></a><span data-ttu-id="7bd01-270">Elemento DefiningExpression (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-270">DefiningExpression Element (CSDL)</span></span>

<span data-ttu-id="7bd01-271">Il **DefiningExpression** elemento schema conceptual schema definition Language (CSDL) contiene un'espressione Entity SQL che definisce una funzione nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-271">The **DefiningExpression** element in conceptual schema definition language (CSDL) contains an Entity SQL expression that defines a function in the conceptual model.</span></span>  

> [!NOTE]
> <span data-ttu-id="7bd01-272">Ai fini della convalida, un **DefiningExpression** elemento può contenere contenuto arbitrario.</span><span class="sxs-lookup"><span data-stu-id="7bd01-272">For validation purposes, a **DefiningExpression** element can contain arbitrary content.</span></span> <span data-ttu-id="7bd01-273">Tuttavia, Entity Framework genererà un'eccezione in fase di esecuzione se un **DefiningExpression** elemento neobsahuje Entity SQL valido.</span><span class="sxs-lookup"><span data-stu-id="7bd01-273">However, Entity Framework will throw an exception at runtime if a **DefiningExpression** element does not contain valid Entity SQL.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-274">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-274">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-275">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **DefiningExpression** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-275">Any number of annotation attributes (custom XML attributes) may be applied to the **DefiningExpression** element.</span></span> <span data-ttu-id="7bd01-276">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-276">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-277">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-277">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="7bd01-278">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-278">Example</span></span>

<span data-ttu-id="7bd01-279">L'esempio seguente usa un' **DefiningExpression** elemento per definire una funzione che restituisce il numero di anni, poiché è stato pubblicato un libro.</span><span class="sxs-lookup"><span data-stu-id="7bd01-279">The following example uses a **DefiningExpression** element to define a function that returns the number of years since a book was published.</span></span> <span data-ttu-id="7bd01-280">Il contenuto di **DefiningExpression** elemento viene scritto in Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-280">The content of the **DefiningExpression** element is written in Entity SQL.</span></span>

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a><span data-ttu-id="7bd01-281">Elemento Dependent (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-281">Dependent Element (CSDL)</span></span>

<span data-ttu-id="7bd01-282">Il **dipendenti** conceptual schema definition Language (CSDL) è un elemento figlio all'elemento ReferentialConstraint e definisce l'entità finale dipendente di un vincolo referenziale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-282">The **Dependent** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element and defines the dependent end of a referential constraint.</span></span> <span data-ttu-id="7bd01-283">Oggetto **ReferentialConstraint** elemento definisce la funzionalità che è simile a un vincolo di integrità referenziale in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-283">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="7bd01-284">Nello stesso modo in cui una colonna (o più colonne) da una tabella di database può fare riferimento alla chiave primaria di un'altra tabella, una proprietà (o più proprietà) di un tipo di entità può fare riferimento alla chiave di entità di un altro tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-284">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="7bd01-285">Il tipo di entità cui viene fatto riferimento viene chiamato il *entità finale principale* del vincolo.</span><span class="sxs-lookup"><span data-stu-id="7bd01-285">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="7bd01-286">Il tipo di entità che fa riferimento l'entità finale principale viene chiamato il *entità finale dipendente* del vincolo.</span><span class="sxs-lookup"><span data-stu-id="7bd01-286">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="7bd01-287">**PropertyRef** gli elementi vengono usati per specificare quali chiavi fanno riferimento l'entità finale principale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-287">**PropertyRef** elements are used to specify which keys reference the principal end.</span></span>

<span data-ttu-id="7bd01-288">Il **dipendenti** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="7bd01-288">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="7bd01-289">PropertyRef (uno o più elementi)</span><span class="sxs-lookup"><span data-stu-id="7bd01-289">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="7bd01-290">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="7bd01-290">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-291">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-291">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-292">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **dipendenti** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-292">The table below describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="7bd01-293">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="7bd01-293">Attribute Name</span></span> | <span data-ttu-id="7bd01-294">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="7bd01-294">Is Required</span></span> | <span data-ttu-id="7bd01-295">Valore</span><span class="sxs-lookup"><span data-stu-id="7bd01-295">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="7bd01-296">**Ruolo**</span><span class="sxs-lookup"><span data-stu-id="7bd01-296">**Role**</span></span>       | <span data-ttu-id="7bd01-297">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-297">Yes</span></span>         | <span data-ttu-id="7bd01-298">Nome del tipo di entità in un'entità finale dipendente dell'associazione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-298">The name of the entity type on the dependent end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="7bd01-299">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **dipendenti** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-299">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="7bd01-300">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-300">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-301">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-301">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="7bd01-302">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-302">Example</span></span>

<span data-ttu-id="7bd01-303">L'esempio seguente mostra una **ReferentialConstraint** elemento utilizzato come parte della definizione del **PublishedBy** association.</span><span class="sxs-lookup"><span data-stu-id="7bd01-303">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="7bd01-304">Il **PublisherId** proprietà delle **libro** tipo di entità costituisce l'entità finale dipendente del vincolo referenziale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-304">The **PublisherId** property of the **Book** entity type makes up the dependent end of the referential constraint.</span></span>

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
 

 

## <a name="documentation-element-csdl"></a><span data-ttu-id="7bd01-305">Elemento Documentation (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-305">Documentation Element (CSDL)</span></span>

<span data-ttu-id="7bd01-306">Il **documentazione** elemento schema conceptual schema definition Language (CSDL) può essere usato per fornire informazioni su un oggetto definito in un elemento padre.</span><span class="sxs-lookup"><span data-stu-id="7bd01-306">The **Documentation** element in conceptual schema definition language (CSDL) can be used to provide information about an object that is defined in a parent element.</span></span> <span data-ttu-id="7bd01-307">In un file con estensione edmx, quando la **documentazione** elemento è figlio di un elemento che appaia come un oggetto nell'area di progettazione della finestra di progettazione di Entity Framework (ad esempio un'entità, associazioni o proprietà), il contenuto del **documentazione**  elemento verrà visualizzato in Visual Studio **proprietà** finestra per l'oggetto.</span><span class="sxs-lookup"><span data-stu-id="7bd01-307">In an .edmx file, when the **Documentation** element is a child of an element that appears as an object on the design surface of the EF Designer  (such as an entity, association, or property), the contents of the **Documentation** element will appear in the Visual Studio **Properties** window for the object.</span></span>

<span data-ttu-id="7bd01-308">Il **documentazione** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="7bd01-308">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="7bd01-309">**Riepilogo**: una breve descrizione dell'elemento padre.</span><span class="sxs-lookup"><span data-stu-id="7bd01-309">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="7bd01-310">Zero o un elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-310">(zero or one element)</span></span>
-   <span data-ttu-id="7bd01-311">**LongDescription**: descrizione Luna dell'elemento padre.</span><span class="sxs-lookup"><span data-stu-id="7bd01-311">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="7bd01-312">Zero o un elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-312">(zero or one element)</span></span>
-   <span data-ttu-id="7bd01-313">Elementi di annotazione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-313">Annotation elements.</span></span> <span data-ttu-id="7bd01-314">Zero o più elementi.</span><span class="sxs-lookup"><span data-stu-id="7bd01-314">(zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-315">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-315">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-316">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **documentazione** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-316">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="7bd01-317">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-317">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-318">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-318">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="7bd01-319">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-319">Example</span></span>

<span data-ttu-id="7bd01-320">L'esempio seguente mostra le **documentazione** elemento come elemento figlio di un elemento EntityType.</span><span class="sxs-lookup"><span data-stu-id="7bd01-320">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span> <span data-ttu-id="7bd01-321">Se il frammento di codice seguente nel file CSDL del contenuto di un'estensione edmx file, il contenuto del **Summary** e **LongDescription** elementi apparirebbe in Visual Studio **proprietà** finestra dei comandi quando si fa clic su di `Customer` tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-321">If the snippet below were in the CSDL content of an .edmx file, the contents of the **Summary** and **LongDescription** elements would appear in the Visual Studio **Properties** window when you click on the `Customer` entity type.</span></span>

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
 

 

## <a name="end-element-csdl"></a><span data-ttu-id="7bd01-322">Elemento End (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-322">End Element (CSDL)</span></span>

<span data-ttu-id="7bd01-323">Il **End** elemento schema conceptual schema definition Language (CSDL) può essere un figlio dell'elemento di associazione o l'elemento AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="7bd01-323">The **End** element in conceptual schema definition language (CSDL) can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="7bd01-324">In ogni caso, il ruolo del **End** elemento è diverso e i relativi attributi applicabili sono diversi.</span><span class="sxs-lookup"><span data-stu-id="7bd01-324">In each case, the role of the **End** element is different and the applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="7bd01-325">Elemento End come figlio dell'elemento Association</span><span class="sxs-lookup"><span data-stu-id="7bd01-325">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="7bd01-326">Un' **finali** elemento (come figlio del **Association** elemento) identifica il tipo di entità in un'estremità di un'associazione e il numero di istanze del tipo di entità che possono essere presenti in tale entità finale di un'associazione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-326">An **End** element (as a child of the **Association** element) identifies the entity type on one end of an association and the number of entity type instances that can exist at that end of an association.</span></span> <span data-ttu-id="7bd01-327">Le entità finali dell'associazione sono definite come parte di un'associazione; un'associazione deve disporre esattamente di due entità finali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-327">Association ends are defined as part of an association; an association must have exactly two association ends.</span></span> <span data-ttu-id="7bd01-328">Le istanze del tipo di entità in corrispondenza di un'entità finale di un'associazione sono accessibili attraverso proprietà di navigazione o chiavi esterne se sono esposte in un tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-328">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys if they are exposed on an entity type.</span></span>  

<span data-ttu-id="7bd01-329">Un' **End** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="7bd01-329">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="7bd01-330">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="7bd01-330">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="7bd01-331">OnDelete (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="7bd01-331">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="7bd01-332">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="7bd01-332">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-333">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-333">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-334">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **finali** elemento quando è figlio di un **associazione** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-334">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="7bd01-335">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="7bd01-335">Attribute Name</span></span>   | <span data-ttu-id="7bd01-336">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="7bd01-336">Is Required</span></span> | <span data-ttu-id="7bd01-337">Valore</span><span class="sxs-lookup"><span data-stu-id="7bd01-337">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="7bd01-338">**Type**</span><span class="sxs-lookup"><span data-stu-id="7bd01-338">**Type**</span></span>         | <span data-ttu-id="7bd01-339">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-339">Yes</span></span>         | <span data-ttu-id="7bd01-340">Nome del tipo di entità in una entità finale dell'associazione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-340">The name of the entity type at one end of the association.</span></span>                                                                                                                                                                                                                                                                                                                                                         |
| <span data-ttu-id="7bd01-341">**Ruolo**</span><span class="sxs-lookup"><span data-stu-id="7bd01-341">**Role**</span></span>         | <span data-ttu-id="7bd01-342">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-342">No</span></span>          | <span data-ttu-id="7bd01-343">Nome per l'entità finale dell'associazione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-343">A name for the association end.</span></span> <span data-ttu-id="7bd01-344">Se non è fornito alcun nome, verrà utilizzato il nome del tipo di entità nell'entità finale dell'associazione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-344">If no name is provided, the name of the entity type on the association end will be used.</span></span>                                                                                                                                                                                                                                                                                           |
| <span data-ttu-id="7bd01-345">**Molteplicità**</span><span class="sxs-lookup"><span data-stu-id="7bd01-345">**Multiplicity**</span></span> | <span data-ttu-id="7bd01-346">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-346">Yes</span></span>         | <span data-ttu-id="7bd01-347">**1**, **0..1**, o **\*** a seconda del numero di istanze del tipo di entità che possono essere alla fine dell'associazione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-347">**1**, **0..1**, or **\*** depending on the number of entity type instances that can be at the end of the association.</span></span> <br/> <span data-ttu-id="7bd01-348">**1** indica tale istanza del tipo esattamente un'entità è presente l'estremità dell'associazione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-348">**1** indicates that exactly one entity type instance exists at the association end.</span></span> <br/> <span data-ttu-id="7bd01-349">**0..1** indica che all'entità finale dell'associazione sono presenti zero o più istanze del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-349">**0..1** indicates that zero or one entity type instances exist at the association end.</span></span> <br/> <span data-ttu-id="7bd01-350">**\*** indica che da zero, uno o più istanze del tipo di entità sono presenti entità finale dell'associazione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-350">**\*** indicates that zero, one, or more entity type instances exist at the association end.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="7bd01-351">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **End** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-351">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="7bd01-352">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-352">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-353">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-353">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="7bd01-354">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-354">Example</span></span>

<span data-ttu-id="7bd01-355">L'esempio seguente mostra un' **Association** elemento che definisce il **CustomerOrders** association.</span><span class="sxs-lookup"><span data-stu-id="7bd01-355">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="7bd01-356">Il **molteplicità** i valori per ogni **finali** dell'associazione indicano che molte **ordini** può essere associato un **cliente**, ma un solo **cliente** può essere associato un **ordine**.</span><span class="sxs-lookup"><span data-stu-id="7bd01-356">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="7bd01-357">Inoltre, il **OnDelete** elemento indica che tutte le **ordini** che sono correlati a un particolare **cliente** e che sono stati caricati in ObjectContext sarà Se eliminato il **cliente** viene eliminato.</span><span class="sxs-lookup"><span data-stu-id="7bd01-357">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and that have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="7bd01-358">Elemento End come figlio dell'elemento AssociationSet</span><span class="sxs-lookup"><span data-stu-id="7bd01-358">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="7bd01-359">Il **End** elemento specifica un'entità finale di un set di associazioni.</span><span class="sxs-lookup"><span data-stu-id="7bd01-359">The **End** element specifies one end of an association set.</span></span> <span data-ttu-id="7bd01-360">Il **AssociationSet** l'elemento deve contenere due **End** elementi.</span><span class="sxs-lookup"><span data-stu-id="7bd01-360">The **AssociationSet** element must contain two **End** elements.</span></span> <span data-ttu-id="7bd01-361">Le informazioni contenute in un' **End** elemento viene usato per eseguire il mapping di un set di associazioni a un'origine dati.</span><span class="sxs-lookup"><span data-stu-id="7bd01-361">The information contained in an **End** element is used in mapping an association set to a data source.</span></span>

<span data-ttu-id="7bd01-362">Un' **End** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="7bd01-362">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="7bd01-363">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="7bd01-363">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="7bd01-364">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="7bd01-364">Annotation elements (zero or more elements)</span></span>

> [!NOTE]
> <span data-ttu-id="7bd01-365">Gli elementi Annotation devono apparire dopo tutti gli altri elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="7bd01-365">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="7bd01-366">Elementi di annotazione sono consentiti solo nella versione 2 CSDL e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="7bd01-366">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-367">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-367">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-368">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **finali** elemento quando è figlio di un **AssociationSet** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-368">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="7bd01-369">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="7bd01-369">Attribute Name</span></span> | <span data-ttu-id="7bd01-370">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="7bd01-370">Is Required</span></span> | <span data-ttu-id="7bd01-371">Valore</span><span class="sxs-lookup"><span data-stu-id="7bd01-371">Value</span></span>                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="7bd01-372">**Elemento EntitySet**</span><span class="sxs-lookup"><span data-stu-id="7bd01-372">**EntitySet**</span></span>  | <span data-ttu-id="7bd01-373">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-373">Yes</span></span>         | <span data-ttu-id="7bd01-374">Il nome del **EntitySet** elemento che definisce un'entità finale dell'elemento padre **AssociationSet** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-374">The name of the **EntitySet** element that defines one end of the parent **AssociationSet** element.</span></span> <span data-ttu-id="7bd01-375">Il **EntitySet** elemento deve essere definito nello stesso contenitore di entità come elemento padre **AssociationSet** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-375">The **EntitySet** element must be defined in the same entity container as the parent **AssociationSet** element.</span></span> |
| <span data-ttu-id="7bd01-376">**Ruolo**</span><span class="sxs-lookup"><span data-stu-id="7bd01-376">**Role**</span></span>       | <span data-ttu-id="7bd01-377">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-377">No</span></span>          | <span data-ttu-id="7bd01-378">Nome dell'entità finale del set di associazioni.</span><span class="sxs-lookup"><span data-stu-id="7bd01-378">The name of the association set end.</span></span> <span data-ttu-id="7bd01-379">Se il **ruolo** attributo non viene utilizzato, il nome del set di entità finale dell'associazione sarà il nome del set di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-379">If the **Role** attribute is not used, the name of the association set end will be the name of the entity set.</span></span>                                                                   |

 

> [!NOTE]
> <span data-ttu-id="7bd01-380">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **End** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-380">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="7bd01-381">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-381">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-382">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-382">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="7bd01-383">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-383">Example</span></span>

<span data-ttu-id="7bd01-384">L'esempio seguente mostra un' **EntityContainer** elemento con due **AssociationSet** elementi, ognuno con due **End** elementi:</span><span class="sxs-lookup"><span data-stu-id="7bd01-384">The following example shows an **EntityContainer** element with two **AssociationSet** elements, each with two **End** elements:</span></span>

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
 

 

## <a name="entitycontainer-element-csdl"></a><span data-ttu-id="7bd01-385">Elemento EntityContainer (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-385">EntityContainer Element (CSDL)</span></span>

<span data-ttu-id="7bd01-386">Il **EntityContainer** elemento schema conceptual schema definition Language (CSDL) è un contenitore logico per set di entità, set di associazioni e importazioni di funzioni.</span><span class="sxs-lookup"><span data-stu-id="7bd01-386">The **EntityContainer** element in conceptual schema definition language (CSDL) is a logical container for entity sets, association sets, and function imports.</span></span> <span data-ttu-id="7bd01-387">Un contenitore di entità del modello concettuale viene mappato a un contenitore di entità modello di archiviazione tramite l'elemento EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="7bd01-387">A conceptual model entity container maps to a storage model entity container through the EntityContainerMapping element.</span></span> <span data-ttu-id="7bd01-388">Un contenitore di entità del modello di archiviazione descrive la struttura del database: i set di entità descrivono le tabelle, i set di associazioni descrivono i vincoli delle chiavi esterne e le importazioni di funzioni descrivono le stored procedure in un database.</span><span class="sxs-lookup"><span data-stu-id="7bd01-388">A storage model entity container describes the structure of the database: entity sets describe tables, association sets describe foreign key constraints, and function imports describe stored procedures in a database.</span></span>

<span data-ttu-id="7bd01-389">Un' **EntityContainer** elemento può avere zero o più elementi di documentazione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-389">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="7bd01-390">Se un **documentazione** l'elemento è presente, deve precedere tutti **EntitySet**, **AssociationSet**, e **FunctionImport** elementi.</span><span class="sxs-lookup"><span data-stu-id="7bd01-390">If a **Documentation** element is present, it must precede all **EntitySet**, **AssociationSet**, and **FunctionImport** elements.</span></span>

<span data-ttu-id="7bd01-391">Un' **EntityContainer** elemento può includere zero o più degli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="7bd01-391">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="7bd01-392">EntitySet</span><span class="sxs-lookup"><span data-stu-id="7bd01-392">EntitySet</span></span>
-   <span data-ttu-id="7bd01-393">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="7bd01-393">AssociationSet</span></span>
-   <span data-ttu-id="7bd01-394">FunctionImport</span><span class="sxs-lookup"><span data-stu-id="7bd01-394">FunctionImport</span></span>
-   <span data-ttu-id="7bd01-395">Elementi Annotation</span><span class="sxs-lookup"><span data-stu-id="7bd01-395">Annotation elements</span></span>

<span data-ttu-id="7bd01-396">È possibile estendere un **EntityContainer** per includere il contenuto di un altro elemento **EntityContainer** che si trova all'interno dello stesso spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="7bd01-396">You can extend an **EntityContainer** element to include the contents of another **EntityContainer** that is within the same namespace.</span></span> <span data-ttu-id="7bd01-397">Per includere il contenuto di un altro **EntityContainer**, il tipo di riferimento **EntityContainer** elemento, impostare il valore della **Extends** attributo sul nome del  **EntityContainer** elemento che si desidera includere.</span><span class="sxs-lookup"><span data-stu-id="7bd01-397">To include the contents of another **EntityContainer**, in the referencing **EntityContainer** element, set the value of the **Extends** attribute to the name of the **EntityContainer** element that you want to include.</span></span> <span data-ttu-id="7bd01-398">Tutti gli elementi figlio di include **EntityContainer** elemento verrà considerato come elementi figlio di fare riferimento alle **EntityContainer** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-398">All child elements of the included **EntityContainer** element will be treated as child elements of the referencing **EntityContainer** element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-399">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-399">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-400">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **Using** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-400">The table below describes the attributes that can be applied to the **Using** element.</span></span>

| <span data-ttu-id="7bd01-401">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="7bd01-401">Attribute Name</span></span> | <span data-ttu-id="7bd01-402">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="7bd01-402">Is Required</span></span> | <span data-ttu-id="7bd01-403">Valore</span><span class="sxs-lookup"><span data-stu-id="7bd01-403">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="7bd01-404">**Name**</span><span class="sxs-lookup"><span data-stu-id="7bd01-404">**Name**</span></span>       | <span data-ttu-id="7bd01-405">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-405">Yes</span></span>         | <span data-ttu-id="7bd01-406">Nome del contenitore di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-406">The name of the entity container.</span></span>                               |
| <span data-ttu-id="7bd01-407">**Estende**</span><span class="sxs-lookup"><span data-stu-id="7bd01-407">**Extends**</span></span>    | <span data-ttu-id="7bd01-408">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-408">No</span></span>          | <span data-ttu-id="7bd01-409">Nome di un altro contenitore di entità all'interno dello stesso spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="7bd01-409">The name of another entity container within the same namespace.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="7bd01-410">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **EntityContainer** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-410">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="7bd01-411">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-411">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-412">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-412">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="7bd01-413">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-413">Example</span></span>

<span data-ttu-id="7bd01-414">L'esempio seguente mostra un' **EntityContainer** elemento che definisce tre set di entità e due set di associazioni.</span><span class="sxs-lookup"><span data-stu-id="7bd01-414">The following example shows an **EntityContainer** element that defines three entity sets and two association sets.</span></span>

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
 

 

## <a name="entityset-element-csdl"></a><span data-ttu-id="7bd01-415">Elemento EntitySet (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-415">EntitySet Element (CSDL)</span></span>

<span data-ttu-id="7bd01-416">Il **EntitySet** elemento di linguaggio conceptual schema definition language è un contenitore logico per istanze di un tipo di entità e le istanze di qualsiasi tipo derivato da tale tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-416">The **EntitySet** element in conceptual schema definition language is a logical container for instances of an entity type and instances of any type that is derived from that entity type.</span></span> <span data-ttu-id="7bd01-417">La relazione tra un tipo di entità e un set di entità è analoga alla relazione tra una riga e una tabella in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-417">The relationship between an entity type and an entity set is analogous to the relationship between a row and a table in a relational database.</span></span> <span data-ttu-id="7bd01-418">Analogamente a una riga, un tipo di entità definisce un set di dati correlati e, analogamente a una tabella, un set di entità contiene istanze di quella definizione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-418">Like a row, an entity type defines a set of related data, and, like a table, an entity set contains instances of that definition.</span></span> <span data-ttu-id="7bd01-419">Un set di entità fornisce un construct per il raggruppamento di istanze del tipo di entità in modo che se ne possa eseguire il mapping alle strutture dei dati correlati in un'origine dati.</span><span class="sxs-lookup"><span data-stu-id="7bd01-419">An entity set provides a construct for grouping entity type instances so that they can be mapped to related data structures in a data source.</span></span>  

<span data-ttu-id="7bd01-420">È possibile definire più set di entità per un particolare tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-420">More than one entity set for a particular entity type may be defined.</span></span>

> [!NOTE]
> <span data-ttu-id="7bd01-421">La finestra di progettazione di Entity Framework non supporta i modelli concettuali che contengono più set di entità per tipo.</span><span class="sxs-lookup"><span data-stu-id="7bd01-421">The EF Designer does not support conceptual models that contain multiple entity sets per type.</span></span>

 

<span data-ttu-id="7bd01-422">Il **EntitySet** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="7bd01-422">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="7bd01-423">Elemento documentation (zero o un elemento consentito)</span><span class="sxs-lookup"><span data-stu-id="7bd01-423">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="7bd01-424">Elementi Annotation (zero o più elementi consentiti)</span><span class="sxs-lookup"><span data-stu-id="7bd01-424">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-425">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-425">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-426">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **EntitySet** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-426">The table below describes the attributes that can be applied to the **EntitySet** element.</span></span>

| <span data-ttu-id="7bd01-427">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="7bd01-427">Attribute Name</span></span> | <span data-ttu-id="7bd01-428">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="7bd01-428">Is Required</span></span> | <span data-ttu-id="7bd01-429">Valore</span><span class="sxs-lookup"><span data-stu-id="7bd01-429">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="7bd01-430">**Name**</span><span class="sxs-lookup"><span data-stu-id="7bd01-430">**Name**</span></span>       | <span data-ttu-id="7bd01-431">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-431">Yes</span></span>         | <span data-ttu-id="7bd01-432">Nome del set di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-432">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="7bd01-433">**Elemento EntityType**</span><span class="sxs-lookup"><span data-stu-id="7bd01-433">**EntityType**</span></span> | <span data-ttu-id="7bd01-434">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-434">Yes</span></span>         | <span data-ttu-id="7bd01-435">Nome completo del tipo di entità per il quale il set di entità contiene delle istanze.</span><span class="sxs-lookup"><span data-stu-id="7bd01-435">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="7bd01-436">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **EntitySet** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-436">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="7bd01-437">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-437">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-438">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-438">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="7bd01-439">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-439">Example</span></span>

<span data-ttu-id="7bd01-440">L'esempio seguente mostra un' **EntityContainer** elemento con tre **EntitySet** elementi:</span><span class="sxs-lookup"><span data-stu-id="7bd01-440">The following example shows an **EntityContainer** element with three **EntitySet** elements:</span></span>

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
 

<span data-ttu-id="7bd01-441">È possibile definire più set di entità per tipo (MEST).</span><span class="sxs-lookup"><span data-stu-id="7bd01-441">It is possible to define multiple entity sets per type (MEST).</span></span> <span data-ttu-id="7bd01-442">L'esempio seguente definisce un contenitore di entità con due set di entità per il **libro** tipo di entità:</span><span class="sxs-lookup"><span data-stu-id="7bd01-442">The following example defines an entity container with two entity sets for the **Book** entity type:</span></span>

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
 

 

## <a name="entitytype-element-csdl"></a><span data-ttu-id="7bd01-443">Elemento EntityType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-443">EntityType Element (CSDL)</span></span>

<span data-ttu-id="7bd01-444">Il **EntityType** elemento rappresenta la struttura di un concetto di livello superiore, ad esempio un cliente o un ordine, in un modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-444">The **EntityType** element represents the structure of a top-level concept, such as a customer or order, in a conceptual model.</span></span> <span data-ttu-id="7bd01-445">Un tipo di entità è un modello per istanze di tipi di entità in un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-445">An entity type is a template for instances of entity types in an application.</span></span> <span data-ttu-id="7bd01-446">Ogni modello contiene le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="7bd01-446">Each template contains the following information:</span></span>

-   <span data-ttu-id="7bd01-447">Un nome univoco.</span><span class="sxs-lookup"><span data-stu-id="7bd01-447">A unique name.</span></span> <span data-ttu-id="7bd01-448">Obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="7bd01-448">(Required.)</span></span>
-   <span data-ttu-id="7bd01-449">Una chiave di entità che è definita da una o più proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-449">An entity key that is defined by one or more properties.</span></span> <span data-ttu-id="7bd01-450">Obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="7bd01-450">(Required.)</span></span>
-   <span data-ttu-id="7bd01-451">Proprietà per contenere dati.</span><span class="sxs-lookup"><span data-stu-id="7bd01-451">Properties for containing data.</span></span> <span data-ttu-id="7bd01-452">(Facoltative)</span><span class="sxs-lookup"><span data-stu-id="7bd01-452">(Optional.)</span></span>
-   <span data-ttu-id="7bd01-453">Proprietà di navigazione che consentono di navigare da un'entità finale di un'associazione all'altra.</span><span class="sxs-lookup"><span data-stu-id="7bd01-453">Navigation properties that allow for navigation from one end of an association to the other end.</span></span> <span data-ttu-id="7bd01-454">(Facoltative)</span><span class="sxs-lookup"><span data-stu-id="7bd01-454">(Optional.)</span></span>

<span data-ttu-id="7bd01-455">In un'applicazione, un'istanza di un tipo di entità rappresenta un oggetto specifico, quale ad esempio un cliente o un ordine specifico.</span><span class="sxs-lookup"><span data-stu-id="7bd01-455">In an application, an instance of an entity type represents a specific object (such as a specific customer or order).</span></span> <span data-ttu-id="7bd01-456">Ogni istanza di un tipo di entità deve avere una chiave di entità univoca all'interno di un set di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-456">Each instance of an entity type must have a unique entity key within an entity set.</span></span>

<span data-ttu-id="7bd01-457">Due istanze di tipi di entità sono considerate uguali solo se sono dello stesso tipo e se i valori delle rispettive chiavi di entità sono uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-457">Two entity type instances are considered equal only if they are of the same type and the values of their entity keys are the same.</span></span>

<span data-ttu-id="7bd01-458">Un' **EntityType** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="7bd01-458">An **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="7bd01-459">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="7bd01-459">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="7bd01-460">Chiave (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="7bd01-460">Key (zero or one element)</span></span>
-   <span data-ttu-id="7bd01-461">Proprietà (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="7bd01-461">Property (zero or more elements)</span></span>
-   <span data-ttu-id="7bd01-462">NavigationProperty (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="7bd01-462">NavigationProperty (zero or more elements)</span></span>
-   <span data-ttu-id="7bd01-463">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="7bd01-463">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-464">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-464">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-465">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **EntityType** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-465">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="7bd01-466">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="7bd01-466">Attribute Name</span></span>                                                                                                                                  | <span data-ttu-id="7bd01-467">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="7bd01-467">Is Required</span></span> | <span data-ttu-id="7bd01-468">Valore</span><span class="sxs-lookup"><span data-stu-id="7bd01-468">Value</span></span>                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="7bd01-469">**Name**</span><span class="sxs-lookup"><span data-stu-id="7bd01-469">**Name**</span></span>                                                                                                                                        | <span data-ttu-id="7bd01-470">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-470">Yes</span></span>         | <span data-ttu-id="7bd01-471">Nome del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-471">The name of the entity type.</span></span>                                                                     |
| <span data-ttu-id="7bd01-472">**BaseType**</span><span class="sxs-lookup"><span data-stu-id="7bd01-472">**BaseType**</span></span>                                                                                                                                    | <span data-ttu-id="7bd01-473">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-473">No</span></span>          | <span data-ttu-id="7bd01-474">Nome di un altro tipo di entità che è il tipo di base del tipo di entità definito.</span><span class="sxs-lookup"><span data-stu-id="7bd01-474">The name of another entity type that is the base type of the entity type that is being defined.</span></span>  |
| <span data-ttu-id="7bd01-475">**Classe astratta**</span><span class="sxs-lookup"><span data-stu-id="7bd01-475">**Abstract**</span></span>                                                                                                                                    | <span data-ttu-id="7bd01-476">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-476">No</span></span>          | <span data-ttu-id="7bd01-477">**True** oppure **False**, a seconda che il tipo di entità sia un tipo astratto.</span><span class="sxs-lookup"><span data-stu-id="7bd01-477">**True** or **False**, depending on whether the entity type is an abstract type.</span></span>                 |
| <span data-ttu-id="7bd01-478">**OpenType**</span><span class="sxs-lookup"><span data-stu-id="7bd01-478">**OpenType**</span></span>                                                                                                                                    | <span data-ttu-id="7bd01-479">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-479">No</span></span>          | <span data-ttu-id="7bd01-480">**True** oppure **False** a seconda che il tipo di entità sia un tipo di entità aperto.</span><span class="sxs-lookup"><span data-stu-id="7bd01-480">**True** or **False** depending on whether the entity type is an open entity type.</span></span> <br/> [!NOTE] |
| <span data-ttu-id="7bd01-481">> Il **OpenType** attributo è applicabile solo ai tipi di entità definiti nei modelli concettuali utilizzati con ADO.NET Data Services.</span><span class="sxs-lookup"><span data-stu-id="7bd01-481">> The **OpenType** attribute is only applicable to entity types that are defined in conceptual models that are used with ADO.NET Data Services.</span></span> |             |                                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="7bd01-482">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **EntityType** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-482">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="7bd01-483">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-483">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-484">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-484">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="7bd01-485">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-485">Example</span></span>

<span data-ttu-id="7bd01-486">L'esempio seguente mostra un' **EntityType** elemento con tre **proprietà** elementi e due **NavigationProperty** elementi:</span><span class="sxs-lookup"><span data-stu-id="7bd01-486">The following example shows an **EntityType** element with three **Property** elements and two **NavigationProperty** elements:</span></span>

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
 

 

## <a name="enumtype-element-csdl"></a><span data-ttu-id="7bd01-487">Elemento EnumType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-487">EnumType Element (CSDL)</span></span>

<span data-ttu-id="7bd01-488">Il **EnumType** elemento rappresenta un tipo enumerato.</span><span class="sxs-lookup"><span data-stu-id="7bd01-488">The **EnumType** element represents an enumerated type.</span></span>

<span data-ttu-id="7bd01-489">Un' **EnumType** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="7bd01-489">An **EnumType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="7bd01-490">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="7bd01-490">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="7bd01-491">Membro (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="7bd01-491">Member (zero or more elements)</span></span>
-   <span data-ttu-id="7bd01-492">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="7bd01-492">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-493">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-493">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-494">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **EnumType** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-494">The table below describes the attributes that can be applied to the **EnumType** element.</span></span>

| <span data-ttu-id="7bd01-495">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="7bd01-495">Attribute Name</span></span>     | <span data-ttu-id="7bd01-496">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="7bd01-496">Is Required</span></span> | <span data-ttu-id="7bd01-497">Valore</span><span class="sxs-lookup"><span data-stu-id="7bd01-497">Value</span></span>                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="7bd01-498">**Name**</span><span class="sxs-lookup"><span data-stu-id="7bd01-498">**Name**</span></span>           | <span data-ttu-id="7bd01-499">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-499">Yes</span></span>         | <span data-ttu-id="7bd01-500">Nome del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-500">The name of the entity type.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="7bd01-501">**IsFlags**</span><span class="sxs-lookup"><span data-stu-id="7bd01-501">**IsFlags**</span></span>        | <span data-ttu-id="7bd01-502">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-502">No</span></span>          | <span data-ttu-id="7bd01-503">**True** oppure **False**, a seconda se il tipo di enumerazione può essere utilizzato come un set di flag.</span><span class="sxs-lookup"><span data-stu-id="7bd01-503">**True** or **False**, depending on whether the enum type can be used as a set of flags.</span></span> <span data-ttu-id="7bd01-504">Il valore predefinito è **False.**.</span><span class="sxs-lookup"><span data-stu-id="7bd01-504">The default value is **False.**.</span></span>                                                                     |
| <span data-ttu-id="7bd01-505">**UnderlyingType**</span><span class="sxs-lookup"><span data-stu-id="7bd01-505">**UnderlyingType**</span></span> | <span data-ttu-id="7bd01-506">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-506">No</span></span>          | <span data-ttu-id="7bd01-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** oppure **Edm.SByte** che definisce l'intervallo di valori del tipo.</span><span class="sxs-lookup"><span data-stu-id="7bd01-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** or **Edm.SByte** defining the range of values of the type.</span></span>   <span data-ttu-id="7bd01-508">È il tipo sottostante predefinito degli elementi di enumerazioni **Edm.Int32.**.</span><span class="sxs-lookup"><span data-stu-id="7bd01-508">The default underlying type of enumeration elements is **Edm.Int32.**.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="7bd01-509">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **EnumType** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-509">Any number of annotation attributes (custom XML attributes) may be applied to the **EnumType** element.</span></span> <span data-ttu-id="7bd01-510">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-510">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-511">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-511">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="7bd01-512">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-512">Example</span></span>

<span data-ttu-id="7bd01-513">L'esempio seguente mostra un' **EnumType** elemento con tre **membro** elementi:</span><span class="sxs-lookup"><span data-stu-id="7bd01-513">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a><span data-ttu-id="7bd01-514">Elemento Function (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-514">Function Element (CSDL)</span></span>

<span data-ttu-id="7bd01-515">Il **funzione** elemento schema conceptual schema definition Language (CSDL) viene usato per definire o dichiarare funzioni nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-515">The **Function** element in conceptual schema definition language (CSDL) is used to define or declare functions in the conceptual model.</span></span> <span data-ttu-id="7bd01-516">Una funzione viene definita utilizzando un elemento DefiningExpression.</span><span class="sxs-lookup"><span data-stu-id="7bd01-516">A function is defined by using a DefiningExpression element.</span></span>  

<span data-ttu-id="7bd01-517">Oggetto **funzione** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="7bd01-517">A **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="7bd01-518">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="7bd01-518">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="7bd01-519">Parametro (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="7bd01-519">Parameter (zero or more elements)</span></span>
-   <span data-ttu-id="7bd01-520">DefiningExpression (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="7bd01-520">DefiningExpression (zero or one element)</span></span>
-   <span data-ttu-id="7bd01-521">ReturnType (funzione) (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="7bd01-521">ReturnType (Function) (zero or one element)</span></span>
-   <span data-ttu-id="7bd01-522">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="7bd01-522">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="7bd01-523">Restituisce un tipo per una funzione deve essere specificato mediante il **ReturnType** (Function) (elemento) o il **ReturnType** attributo (vedere sotto), ma non entrambi.</span><span class="sxs-lookup"><span data-stu-id="7bd01-523">A return type for a function must be specified with either the **ReturnType** (Function) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="7bd01-524">I tipi restituiti possibili sono il edmSimpleType, il tipo di entità, il tipo complesso, il tipo di riga o il tipo di riferimento o una raccolta di uno di questi tipi.</span><span class="sxs-lookup"><span data-stu-id="7bd01-524">The possible return types are any EdmSimpleType, entity type, complex type, row type, or ref type (or a collection of one of these types).</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-525">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-525">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-526">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **funzione** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-526">The table below describes the attributes that can be applied to the **Function** element.</span></span>

| <span data-ttu-id="7bd01-527">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="7bd01-527">Attribute Name</span></span> | <span data-ttu-id="7bd01-528">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="7bd01-528">Is Required</span></span> | <span data-ttu-id="7bd01-529">Valore</span><span class="sxs-lookup"><span data-stu-id="7bd01-529">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="7bd01-530">**Name**</span><span class="sxs-lookup"><span data-stu-id="7bd01-530">**Name**</span></span>       | <span data-ttu-id="7bd01-531">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-531">Yes</span></span>         | <span data-ttu-id="7bd01-532">Nome della funzione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-532">The name of the function.</span></span>          |
| <span data-ttu-id="7bd01-533">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="7bd01-533">**ReturnType**</span></span> | <span data-ttu-id="7bd01-534">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-534">No</span></span>          | <span data-ttu-id="7bd01-535">Tipo restituito dalla funzione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-535">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="7bd01-536">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **funzione** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-536">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="7bd01-537">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-537">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-538">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-538">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="7bd01-539">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-539">Example</span></span>

<span data-ttu-id="7bd01-540">L'esempio seguente usa un' **funzione** elemento per definire una funzione che restituisce il numero di anni dopo è stato assunto un docente.</span><span class="sxs-lookup"><span data-stu-id="7bd01-540">The following example uses a **Function** element to define a function that returns the number of years since an instructor was hired.</span></span>

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a><span data-ttu-id="7bd01-541">Elemento FunctionImport (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-541">FunctionImport Element (CSDL)</span></span>

<span data-ttu-id="7bd01-542">Il **FunctionImport** elemento schema conceptual schema definition Language (CSDL) rappresenta una funzione che viene definito nell'origine dati, ma è disponibile per gli oggetti tramite il modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-542">The **FunctionImport** element in conceptual schema definition language (CSDL) represents a function that is defined in the data source but available to objects through the conceptual model.</span></span> <span data-ttu-id="7bd01-543">Ad esempio, un elemento di funzione nel modello di archiviazione è utilizzabile per rappresentare una stored procedure in un database.</span><span class="sxs-lookup"><span data-stu-id="7bd01-543">For example, a Function element in the storage model can be used to represent a stored procedure in a database.</span></span> <span data-ttu-id="7bd01-544">Oggetto **FunctionImport** elemento nel modello concettuale rappresenta la funzione corrispondente in un'applicazione Entity Framework e viene eseguito il mapping alla funzione del modello di archiviazione usando l'elemento FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="7bd01-544">A **FunctionImport** element in the conceptual model represents the corresponding function in an Entity Framework application and is mapped to the storage model function by using the FunctionImportMapping element.</span></span> <span data-ttu-id="7bd01-545">Quando la funzione viene chiamata nell'applicazione, la stored procedure corrispondente viene eseguita nel database.</span><span class="sxs-lookup"><span data-stu-id="7bd01-545">When the function is called in the application, the corresponding stored procedure is executed in the database.</span></span>

<span data-ttu-id="7bd01-546">Il **FunctionImport** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="7bd01-546">The **FunctionImport** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="7bd01-547">Documentazione (zero o un elemento consentito)</span><span class="sxs-lookup"><span data-stu-id="7bd01-547">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="7bd01-548">Parametro (zero o più elementi consentiti)</span><span class="sxs-lookup"><span data-stu-id="7bd01-548">Parameter (zero or more elements allowed)</span></span>
-   <span data-ttu-id="7bd01-549">Elementi Annotation (zero o più elementi consentiti)</span><span class="sxs-lookup"><span data-stu-id="7bd01-549">Annotation elements (zero or more elements allowed)</span></span>
-   <span data-ttu-id="7bd01-550">ReturnType (FunctionImport) (zero o più elementi consentiti)</span><span class="sxs-lookup"><span data-stu-id="7bd01-550">ReturnType (FunctionImport) (zero or more elements allowed)</span></span>

<span data-ttu-id="7bd01-551">Uno **parametro** elemento deve essere definito per ogni parametro accettati dalla funzione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-551">One **Parameter** element should be defined for each parameter that the function accepts.</span></span>

<span data-ttu-id="7bd01-552">Restituisce un tipo per una funzione deve essere specificato mediante il **ReturnType** elemento (FunctionImport) o il **ReturnType** attributo (vedere sotto), ma non entrambi.</span><span class="sxs-lookup"><span data-stu-id="7bd01-552">A return type for a function must be specified with either the **ReturnType** (FunctionImport) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="7bd01-553">Il valore del tipo restituito deve essere una raccolta di ComplexType, EntityType o EdmSimpleType.</span><span class="sxs-lookup"><span data-stu-id="7bd01-553">The return type value must be a  collection of EdmSimpleType, EntityType, or ComplexType.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-554">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-554">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-555">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **FunctionImport** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-555">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="7bd01-556">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="7bd01-556">Attribute Name</span></span>   | <span data-ttu-id="7bd01-557">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="7bd01-557">Is Required</span></span> | <span data-ttu-id="7bd01-558">Valore</span><span class="sxs-lookup"><span data-stu-id="7bd01-558">Value</span></span>                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="7bd01-559">**Name**</span><span class="sxs-lookup"><span data-stu-id="7bd01-559">**Name**</span></span>         | <span data-ttu-id="7bd01-560">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-560">Yes</span></span>         | <span data-ttu-id="7bd01-561">Nome della funzione importata.</span><span class="sxs-lookup"><span data-stu-id="7bd01-561">The name of the imported function.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="7bd01-562">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="7bd01-562">**ReturnType**</span></span>   | <span data-ttu-id="7bd01-563">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-563">No</span></span>          | <span data-ttu-id="7bd01-564">Tipo restituito dalla funzione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-564">The type that the function returns.</span></span> <span data-ttu-id="7bd01-565">Non utilizzare questo attributo se la funzione non restituisce un valore.</span><span class="sxs-lookup"><span data-stu-id="7bd01-565">Do not use this attribute if the function does not return a value.</span></span> <span data-ttu-id="7bd01-566">In caso contrario, il valore deve essere una raccolta di ComplexType, EntityType o EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="7bd01-566">Otherwise, the value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>        |
| <span data-ttu-id="7bd01-567">**Elemento EntitySet**</span><span class="sxs-lookup"><span data-stu-id="7bd01-567">**EntitySet**</span></span>    | <span data-ttu-id="7bd01-568">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-568">No</span></span>          | <span data-ttu-id="7bd01-569">Se la funzione restituisce una raccolta di entità dei tipi, il valore della **EntitySet** deve essere il set di entità a cui appartiene la raccolta.</span><span class="sxs-lookup"><span data-stu-id="7bd01-569">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="7bd01-570">In caso contrario, il **EntitySet** attributo non deve essere usato.</span><span class="sxs-lookup"><span data-stu-id="7bd01-570">Otherwise, the **EntitySet** attribute must not be used.</span></span> |
| <span data-ttu-id="7bd01-571">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="7bd01-571">**IsComposable**</span></span> | <span data-ttu-id="7bd01-572">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-572">No</span></span>          | <span data-ttu-id="7bd01-573">Se il valore è impostato su true, la funzione è componibile (Table-valued Function) e può essere usata in una query LINQ.</span><span class="sxs-lookup"><span data-stu-id="7bd01-573">If the value is set to true, the function is composable (Table-valued Function) and can be used in a LINQ query.</span></span>  <span data-ttu-id="7bd01-574">Il valore predefinito è **false**.</span><span class="sxs-lookup"><span data-stu-id="7bd01-574">The default is **false**.</span></span>                                                           |

 

> [!NOTE]
> <span data-ttu-id="7bd01-575">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **FunctionImport** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-575">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="7bd01-576">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-576">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-577">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-577">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="7bd01-578">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-578">Example</span></span>

<span data-ttu-id="7bd01-579">L'esempio seguente mostra una **FunctionImport** elemento che accetta un parametro e restituisce una raccolta di tipi di entità:</span><span class="sxs-lookup"><span data-stu-id="7bd01-579">The following example shows a **FunctionImport** element that accepts one parameter and returns a collection of entity types:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a><span data-ttu-id="7bd01-580">Elemento Key (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-580">Key Element (CSDL)</span></span>

<span data-ttu-id="7bd01-581">Il **Key** è un elemento figlio dell'elemento EntityType e definisce un *chiave di entità* (una proprietà o un set di proprietà di un tipo di entità che determinano l'identità).</span><span class="sxs-lookup"><span data-stu-id="7bd01-581">The **Key** element is a child element of the EntityType element and defines an *entity key* (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="7bd01-582">Le proprietà che costituiscono una chiave di entità vengono scelte in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-582">The properties that make up an entity key are chosen at design time.</span></span> <span data-ttu-id="7bd01-583">I valori delle proprietà della chiave di entità devono identificare in modo univoco in fase di esecuzione un'istanza del tipo di entità all'interno di un set di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-583">The values of entity key properties must uniquely identify an entity type instance within an entity set at run time.</span></span> <span data-ttu-id="7bd01-584">Le proprietà che costituiscono una chiave di entità devono essere scelte per garantire univocità delle istanze in un set di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-584">The properties that make up an entity key should be chosen to guarantee uniqueness of instances in an entity set.</span></span> <span data-ttu-id="7bd01-585">Il **chiave** elemento definisce una chiave di entità facendo riferimento a uno o più delle proprietà di un tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-585">The **Key** element defines an entity key by referencing one or more of the properties of an entity type.</span></span>

<span data-ttu-id="7bd01-586">Il **chiave** elemento può avere elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="7bd01-586">The **Key** element can have the following child elements:</span></span>

-   <span data-ttu-id="7bd01-587">PropertyRef (uno o più elementi)</span><span class="sxs-lookup"><span data-stu-id="7bd01-587">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="7bd01-588">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="7bd01-588">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-589">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-589">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-590">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **chiave** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-590">Any number of annotation attributes (custom XML attributes) may be applied to the **Key** element.</span></span> <span data-ttu-id="7bd01-591">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-591">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-592">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-592">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="7bd01-593">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-593">Example</span></span>

<span data-ttu-id="7bd01-594">L'esempio seguente definisce un tipo di entità denominato **libro**.</span><span class="sxs-lookup"><span data-stu-id="7bd01-594">The example below defines an entity type named **Book**.</span></span> <span data-ttu-id="7bd01-595">La chiave di entità viene definita facendo il **ISBN** proprietà del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-595">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="7bd01-596">Il **ISBN** proprietà è una buona scelta per la chiave di entità perché un libro numero ISBN (International Standard) che identifica in modo univoco un libro.</span><span class="sxs-lookup"><span data-stu-id="7bd01-596">The **ISBN** property is a good choice for the entity key because an International Standard Book Number (ISBN) uniquely identifies a book.</span></span>

<span data-ttu-id="7bd01-597">L'esempio seguente mostra un tipo di entità (**Author**) che ha una chiave di entità costituita da due proprietà, **Name** e **indirizzo**.</span><span class="sxs-lookup"><span data-stu-id="7bd01-597">The following example shows an entity type (**Author**) that has an entity key that consists of two properties, **Name** and **Address**.</span></span>

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
 

<span data-ttu-id="7bd01-598">Usando **Name** e **indirizzo** per l'entità chiave è una scelta ragionevole, perché due autori con lo stesso nome vivano allo stesso indirizzo.</span><span class="sxs-lookup"><span data-stu-id="7bd01-598">Using **Name** and **Address** for the entity key is a reasonable choice, because two authors of the same name are unlikely to live at the same address.</span></span> <span data-ttu-id="7bd01-599">Questa scelta per una chiave di entità non garantisce tuttavia in modo assoluto chiavi di entità univoche in un set di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-599">However, this choice for an entity key does not absolutely guarantee unique entity keys in an entity set.</span></span> <span data-ttu-id="7bd01-600">Aggiunta di una proprietà, ad esempio **AuthorId**, che può essere utilizzato per identificare in modo univoco un autore è consigliato in questo caso.</span><span class="sxs-lookup"><span data-stu-id="7bd01-600">Adding a property, such as **AuthorId**, that could be used to uniquely identify an author would be recommended in this case.</span></span>

 

## <a name="member-element-csdl"></a><span data-ttu-id="7bd01-601">Elemento member (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-601">Member Element (CSDL)</span></span>

<span data-ttu-id="7bd01-602">Il **membro** elemento è un elemento figlio dell'elemento EnumType e definisce un membro del tipo enumerato.</span><span class="sxs-lookup"><span data-stu-id="7bd01-602">The **Member** element is a child element of the EnumType element and defines a member of the enumerated type.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-603">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-603">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-604">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **FunctionImport** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-604">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="7bd01-605">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="7bd01-605">Attribute Name</span></span> | <span data-ttu-id="7bd01-606">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="7bd01-606">Is Required</span></span> | <span data-ttu-id="7bd01-607">Valore</span><span class="sxs-lookup"><span data-stu-id="7bd01-607">Value</span></span>                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="7bd01-608">**Name**</span><span class="sxs-lookup"><span data-stu-id="7bd01-608">**Name**</span></span>       | <span data-ttu-id="7bd01-609">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-609">Yes</span></span>         | <span data-ttu-id="7bd01-610">Nome del membro.</span><span class="sxs-lookup"><span data-stu-id="7bd01-610">The name of the member.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="7bd01-611">**Valore**</span><span class="sxs-lookup"><span data-stu-id="7bd01-611">**Value**</span></span>      | <span data-ttu-id="7bd01-612">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-612">No</span></span>          | <span data-ttu-id="7bd01-613">Valore del membro.</span><span class="sxs-lookup"><span data-stu-id="7bd01-613">The value of the member.</span></span> <span data-ttu-id="7bd01-614">Per impostazione predefinita, il primo membro è il valore 0 e il valore di ogni enumeratore successivo viene incrementato di 1.</span><span class="sxs-lookup"><span data-stu-id="7bd01-614">By default, the first member has the value 0, and the value of each successive enumerator is incremented by 1.</span></span> <span data-ttu-id="7bd01-615">Possono esistere più membri con gli stessi valori.</span><span class="sxs-lookup"><span data-stu-id="7bd01-615">Multiple members with the same values may exist.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="7bd01-616">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **FunctionImport** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-616">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="7bd01-617">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-617">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-618">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-618">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="7bd01-619">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-619">Example</span></span>

<span data-ttu-id="7bd01-620">L'esempio seguente mostra un' **EnumType** elemento con tre **membro** elementi:</span><span class="sxs-lookup"><span data-stu-id="7bd01-620">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a><span data-ttu-id="7bd01-621">Elemento NavigationProperty (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-621">NavigationProperty Element (CSDL)</span></span>

<span data-ttu-id="7bd01-622">Oggetto **NavigationProperty** elemento definisce una proprietà di navigazione, che fornisce un riferimento a altra estremità di un'associazione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-622">A **NavigationProperty** element defines a navigation property, which provides a reference to the other end of an association.</span></span> <span data-ttu-id="7bd01-623">A differenza delle proprietà definite con l'elemento proprietà, le proprietà di navigazione non definiscono la forma e le caratteristiche dei dati.</span><span class="sxs-lookup"><span data-stu-id="7bd01-623">Unlike properties defined with the Property element, navigation properties do not define the shape and characteristics of data.</span></span> <span data-ttu-id="7bd01-624">Forniscono un modo per navigare in un'associazione tra due tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-624">They provide a way to navigate an association between two entity types.</span></span>

<span data-ttu-id="7bd01-625">Si noti che le proprietà di navigazione sono facoltative in entrambi tipi di entità nelle entità finali di un'associazione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-625">Note that navigation properties are optional on both entity types at the ends of an association.</span></span> <span data-ttu-id="7bd01-626">Se si definisce una proprietà di navigazione su un tipo di entità nell'entità finale di un'associazione, non è necessario definire una proprietà di navigazione sul tipo di entità nell'altra entità finale dell'associazione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-626">If you define a navigation property on one entity type at the end of an association, you do not have to define a navigation property on the entity type at the other end of the association.</span></span>

<span data-ttu-id="7bd01-627">Il tipo di dati restituito da una proprietà di navigazione è determinato dalla molteplicità della relativa entità finale di associazione remota.</span><span class="sxs-lookup"><span data-stu-id="7bd01-627">The data type returned by a navigation property is determined by the multiplicity of its remote association end.</span></span> <span data-ttu-id="7bd01-628">Ad esempio, si supponga che una proprietà di navigazione **OrdersNavProp**, esiste in un **cliente** tipo di entità e si sposta un'associazione uno-a-molti tra **cliente** e  **Ordine**.</span><span class="sxs-lookup"><span data-stu-id="7bd01-628">For example, suppose a navigation property, **OrdersNavProp**, exists on a **Customer** entity type and navigates a one-to-many association between **Customer** and **Order**.</span></span> <span data-ttu-id="7bd01-629">Poiché l'estremità dell'associazione remota per la proprietà di navigazione con molteplicità molti (\*), il tipo di dati è una raccolta (di **ordine**).</span><span class="sxs-lookup"><span data-stu-id="7bd01-629">Because the remote association end for the navigation property has multiplicity many (\*), its data type is a collection (of **Order**).</span></span> <span data-ttu-id="7bd01-630">Allo stesso modo, se una proprietà di navigazione **CustomerNavProp**, sia disponibile la **ordine** tipo di entità, il tipo di dati sarebbe **cliente** poiché la molteplicità dell'entità finale remota è uno (1).</span><span class="sxs-lookup"><span data-stu-id="7bd01-630">Similarly, if a navigation property, **CustomerNavProp**, exists on the **Order** entity type, its data type would be **Customer** since the multiplicity of the remote end is one (1).</span></span>

<span data-ttu-id="7bd01-631">Oggetto **NavigationProperty** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="7bd01-631">A **NavigationProperty** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="7bd01-632">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="7bd01-632">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="7bd01-633">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="7bd01-633">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-634">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-634">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-635">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **NavigationProperty** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-635">The table below describes the attributes that can be applied to the **NavigationProperty** element.</span></span>

| <span data-ttu-id="7bd01-636">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="7bd01-636">Attribute Name</span></span>   | <span data-ttu-id="7bd01-637">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="7bd01-637">Is Required</span></span> | <span data-ttu-id="7bd01-638">Valore</span><span class="sxs-lookup"><span data-stu-id="7bd01-638">Value</span></span>                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="7bd01-639">**Name**</span><span class="sxs-lookup"><span data-stu-id="7bd01-639">**Name**</span></span>         | <span data-ttu-id="7bd01-640">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-640">Yes</span></span>         | <span data-ttu-id="7bd01-641">Nome della proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-641">The name of the navigation property.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="7bd01-642">**Relazione**</span><span class="sxs-lookup"><span data-stu-id="7bd01-642">**Relationship**</span></span> | <span data-ttu-id="7bd01-643">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-643">Yes</span></span>         | <span data-ttu-id="7bd01-644">Nome di un'associazione che è all'interno dell'ambito del modello.</span><span class="sxs-lookup"><span data-stu-id="7bd01-644">The name of an association that is within the scope of the model.</span></span>                                                                                                                                                                                |
| <span data-ttu-id="7bd01-645">**ToRole**</span><span class="sxs-lookup"><span data-stu-id="7bd01-645">**ToRole**</span></span>       | <span data-ttu-id="7bd01-646">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-646">Yes</span></span>         | <span data-ttu-id="7bd01-647">Entità finale dell'associazione in corrispondenza della quale termina la navigazione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-647">The end of the association at which navigation ends.</span></span> <span data-ttu-id="7bd01-648">Il valore della **ToRole** attributo deve essere identico al valore di uno delle **ruolo** attributi definiti in una delle entità finali dell'associazione (definite nell'elemento AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="7bd01-648">The value of the **ToRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span>       |
| <span data-ttu-id="7bd01-649">**FromRole**</span><span class="sxs-lookup"><span data-stu-id="7bd01-649">**FromRole**</span></span>     | <span data-ttu-id="7bd01-650">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-650">Yes</span></span>         | <span data-ttu-id="7bd01-651">Entità finale dell'associazione dalla quale ha inizio la navigazione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-651">The end of the association from which navigation begins.</span></span> <span data-ttu-id="7bd01-652">Il valore della **FromRole** attributo deve essere identico al valore di uno delle **ruolo** attributi definiti in una delle entità finali dell'associazione (definite nell'elemento AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="7bd01-652">The value of the **FromRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="7bd01-653">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **NavigationProperty** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-653">Any number of annotation attributes (custom XML attributes) may be applied to the **NavigationProperty** element.</span></span> <span data-ttu-id="7bd01-654">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-654">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-655">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-655">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="7bd01-656">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-656">Example</span></span>

<span data-ttu-id="7bd01-657">L'esempio seguente definisce un tipo di entità (**libro**) con due proprietà di navigazione (**PublishedBy** e **WrittenBy**):</span><span class="sxs-lookup"><span data-stu-id="7bd01-657">The following example defines an entity type (**Book**) with two navigation properties (**PublishedBy** and **WrittenBy**):</span></span>

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
 

 

## <a name="ondelete-element-csdl"></a><span data-ttu-id="7bd01-658">Elemento OnDelete (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-658">OnDelete Element (CSDL)</span></span>

<span data-ttu-id="7bd01-659">Il **OnDelete** elemento schema conceptual schema definition Language (CSDL) definisce il comportamento che è connesso a un'associazione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-659">The **OnDelete** element in conceptual schema definition language (CSDL) defines behavior that is connected with an association.</span></span> <span data-ttu-id="7bd01-660">Se il **azione** attributo è impostato su **Cascade** sul uno lato di un'associazione, relativi tipi di entità in altra estremità dell'associazione vengono eliminati quando viene eliminato il tipo di entità nella prima entità finale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-660">If the **Action** attribute is set to **Cascade** on one end of an association, related entity types on the other end of the association are deleted when the entity type on the first end is deleted.</span></span> <span data-ttu-id="7bd01-661">Se l'associazione tra due tipi di entità è una relazione di chiave primaria di chiave-a-primary, allora un oggetto dipendente caricato viene eliminato quando l'oggetto principale in altra estremità dell'associazione viene eliminato indipendentemente i **OnDelete** specifica.</span><span class="sxs-lookup"><span data-stu-id="7bd01-661">If the association between two entity types is a primary key-to-primary key relationship, then a loaded dependent object is deleted when the principal object on the other end of the association is deleted regardless of the **OnDelete** specification.</span></span>  

> [!NOTE]
> <span data-ttu-id="7bd01-662">Il **OnDelete** elemento interessa solo il comportamento di runtime di un'applicazione, e non influisce sul comportamento nell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="7bd01-662">The **OnDelete** element only affects the runtime behavior of an application; it does not affect behavior in the data source.</span></span> <span data-ttu-id="7bd01-663">Il comportamento definito nell'origine dati deve essere uguale al comportamento definito nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-663">The behavior defined in the data source should be the same as the behavior defined in the application.</span></span>

 

<span data-ttu-id="7bd01-664">Un' **OnDelete** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="7bd01-664">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="7bd01-665">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="7bd01-665">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="7bd01-666">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="7bd01-666">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-667">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-667">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-668">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **OnDelete** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-668">The table below describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="7bd01-669">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="7bd01-669">Attribute Name</span></span> | <span data-ttu-id="7bd01-670">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="7bd01-670">Is Required</span></span> | <span data-ttu-id="7bd01-671">Valore</span><span class="sxs-lookup"><span data-stu-id="7bd01-671">Value</span></span>                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="7bd01-672">**Azione**</span><span class="sxs-lookup"><span data-stu-id="7bd01-672">**Action**</span></span>     | <span data-ttu-id="7bd01-673">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-673">Yes</span></span>         | <span data-ttu-id="7bd01-674">**CASCADE** oppure **None**.</span><span class="sxs-lookup"><span data-stu-id="7bd01-674">**Cascade** or **None**.</span></span> <span data-ttu-id="7bd01-675">Se **Cascade**, tipi di entità dipendenti verranno eliminati quando il tipo di entità principale verrà eliminato.</span><span class="sxs-lookup"><span data-stu-id="7bd01-675">If **Cascade**, dependent entity types will be deleted when the principal entity type is deleted.</span></span> <span data-ttu-id="7bd01-676">Se **None**, tipi di entità dipendenti non verranno eliminati quando il tipo di entità principale verrà eliminato.</span><span class="sxs-lookup"><span data-stu-id="7bd01-676">If **None**, dependent entity types will not be deleted when the principal entity type is deleted.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="7bd01-677">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **Association** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-677">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="7bd01-678">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-678">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-679">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-679">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="7bd01-680">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-680">Example</span></span>

<span data-ttu-id="7bd01-681">L'esempio seguente mostra un' **Association** elemento che definisce il **CustomerOrders** association.</span><span class="sxs-lookup"><span data-stu-id="7bd01-681">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="7bd01-682">Il **OnDelete** elemento indica che tutte le **ordini** che sono correlati a un particolare **cliente** e sono stati caricati in ObjectContext saranno eliminati quando il  **Cliente** viene eliminato.</span><span class="sxs-lookup"><span data-stu-id="7bd01-682">The **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted when the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a><span data-ttu-id="7bd01-683">Elemento Parameter (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-683">Parameter Element (CSDL)</span></span>

<span data-ttu-id="7bd01-684">Il **parametro** elemento schema conceptual schema definition Language (CSDL) può essere un figlio dell'elemento FunctionImport o l'elemento di funzione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-684">The **Parameter** element in conceptual schema definition language (CSDL) can be a child of the FunctionImport element or the Function element.</span></span>

### <a name="functionimport-element-application"></a><span data-ttu-id="7bd01-685">Applicazione dell'elemento FunctionImport</span><span class="sxs-lookup"><span data-stu-id="7bd01-685">FunctionImport Element Application</span></span>

<span data-ttu-id="7bd01-686">Oggetto **parametro** elemento (come figlio delle **FunctionImport** elemento) viene usato per definire i parametri di input e outpui per le importazioni di funzioni dichiarate in CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-686">A **Parameter** element (as a child of the **FunctionImport** element) is used to define input and output parameters for function imports that are declared in CSDL.</span></span>

<span data-ttu-id="7bd01-687">Il **parametro** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="7bd01-687">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="7bd01-688">Documentazione (zero o un elemento consentito)</span><span class="sxs-lookup"><span data-stu-id="7bd01-688">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="7bd01-689">Elementi Annotation (zero o più elementi consentiti)</span><span class="sxs-lookup"><span data-stu-id="7bd01-689">Annotation elements (zero or more elements allowed)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-690">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-690">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-691">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **parametro** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-691">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="7bd01-692">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="7bd01-692">Attribute Name</span></span> | <span data-ttu-id="7bd01-693">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="7bd01-693">Is Required</span></span> | <span data-ttu-id="7bd01-694">Valore</span><span class="sxs-lookup"><span data-stu-id="7bd01-694">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="7bd01-695">**Name**</span><span class="sxs-lookup"><span data-stu-id="7bd01-695">**Name**</span></span>       | <span data-ttu-id="7bd01-696">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-696">Yes</span></span>         | <span data-ttu-id="7bd01-697">Nome del parametro.</span><span class="sxs-lookup"><span data-stu-id="7bd01-697">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="7bd01-698">**Type**</span><span class="sxs-lookup"><span data-stu-id="7bd01-698">**Type**</span></span>       | <span data-ttu-id="7bd01-699">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-699">Yes</span></span>         | <span data-ttu-id="7bd01-700">Tipo del parametro.</span><span class="sxs-lookup"><span data-stu-id="7bd01-700">The parameter type.</span></span> <span data-ttu-id="7bd01-701">Il valore deve essere un' **EDMSimpleType** o un tipo complesso che rientra nell'ambito del modello.</span><span class="sxs-lookup"><span data-stu-id="7bd01-701">The value must be an **EDMSimpleType** or a complex type that is within the scope of the model.</span></span>                                                                                                             |
| <span data-ttu-id="7bd01-702">**Modalità**</span><span class="sxs-lookup"><span data-stu-id="7bd01-702">**Mode**</span></span>       | <span data-ttu-id="7bd01-703">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-703">No</span></span>          | <span data-ttu-id="7bd01-704">**Nelle**, **Out**, o **InOut** a seconda che il parametro sia un input, output o parametro di input/output.</span><span class="sxs-lookup"><span data-stu-id="7bd01-704">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="7bd01-705">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="7bd01-705">**MaxLength**</span></span>  | <span data-ttu-id="7bd01-706">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-706">No</span></span>          | <span data-ttu-id="7bd01-707">Lunghezza massima consentita del parametro.</span><span class="sxs-lookup"><span data-stu-id="7bd01-707">The maximum allowed length of the parameter.</span></span>                                                                                                                                                                                    |
| <span data-ttu-id="7bd01-708">**Precisione**</span><span class="sxs-lookup"><span data-stu-id="7bd01-708">**Precision**</span></span>  | <span data-ttu-id="7bd01-709">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-709">No</span></span>          | <span data-ttu-id="7bd01-710">Precisione del parametro.</span><span class="sxs-lookup"><span data-stu-id="7bd01-710">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="7bd01-711">**Scala**</span><span class="sxs-lookup"><span data-stu-id="7bd01-711">**Scale**</span></span>      | <span data-ttu-id="7bd01-712">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-712">No</span></span>          | <span data-ttu-id="7bd01-713">Scala del parametro.</span><span class="sxs-lookup"><span data-stu-id="7bd01-713">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="7bd01-714">**SRID**</span><span class="sxs-lookup"><span data-stu-id="7bd01-714">**SRID**</span></span>       | <span data-ttu-id="7bd01-715">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-715">No</span></span>          | <span data-ttu-id="7bd01-716">Identificatore di riferimento spaziale del sistema.</span><span class="sxs-lookup"><span data-stu-id="7bd01-716">Spatial System Reference Identifier.</span></span> <span data-ttu-id="7bd01-717">Valido solo per i parametri di tipi spaziali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-717">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="7bd01-718">Per altre informazioni, vedere [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="7bd01-718">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="7bd01-719">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **parametro** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-719">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="7bd01-720">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-720">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-721">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-721">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="7bd01-722">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-722">Example</span></span>

<span data-ttu-id="7bd01-723">L'esempio seguente mostra una **FunctionImport** elemento con uno **parametro** elemento figlio.</span><span class="sxs-lookup"><span data-stu-id="7bd01-723">The following example shows a **FunctionImport** element with one **Parameter** child element.</span></span> <span data-ttu-id="7bd01-724">La funzione accetta un parametro di input e restituisce una raccolta di tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-724">The function accepts one input parameter and returns a collection of entity types.</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a><span data-ttu-id="7bd01-725">Applicazione dell'elemento Function</span><span class="sxs-lookup"><span data-stu-id="7bd01-725">Function Element Application</span></span>

<span data-ttu-id="7bd01-726">Oggetto **parametro** elemento (come figlio del **funzione** elemento) Definisce parametri per le funzioni che sono definite o dichiarate in un modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-726">A **Parameter** element (as a child of the **Function** element) defines parameters for functions that are defined or declared in a conceptual model.</span></span>

<span data-ttu-id="7bd01-727">Il **parametro** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="7bd01-727">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="7bd01-728">Documentazione (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="7bd01-728">Documentation (zero or one elements)</span></span>
-   <span data-ttu-id="7bd01-729">Elemento CollectionType (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="7bd01-729">CollectionType (zero or one elements)</span></span>
-   <span data-ttu-id="7bd01-730">ReferenceType (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="7bd01-730">ReferenceType (zero or one elements)</span></span>
-   <span data-ttu-id="7bd01-731">RowType (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="7bd01-731">RowType (zero or one elements)</span></span>

> [!NOTE]
> <span data-ttu-id="7bd01-732">Uno solo dei **CollectionType**, **ReferenceType**, o **RowType** gli elementi possono essere un elemento figlio di un **proprietà** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-732">Only one of the **CollectionType**, **ReferenceType**, or **RowType** elements can be a child element of a **Property** element.</span></span>

 

-   <span data-ttu-id="7bd01-733">Elementi Annotation (zero o più elementi consentiti)</span><span class="sxs-lookup"><span data-stu-id="7bd01-733">Annotation elements (zero or more elements allowed)</span></span>

> [!NOTE]
> <span data-ttu-id="7bd01-734">Gli elementi Annotation devono apparire dopo tutti gli altri elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="7bd01-734">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="7bd01-735">Elementi di annotazione sono consentiti solo nella versione 2 CSDL e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="7bd01-735">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-736">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-736">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-737">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **parametro** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-737">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="7bd01-738">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="7bd01-738">Attribute Name</span></span>   | <span data-ttu-id="7bd01-739">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="7bd01-739">Is Required</span></span> | <span data-ttu-id="7bd01-740">Valore</span><span class="sxs-lookup"><span data-stu-id="7bd01-740">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="7bd01-741">**Name**</span><span class="sxs-lookup"><span data-stu-id="7bd01-741">**Name**</span></span>         | <span data-ttu-id="7bd01-742">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-742">Yes</span></span>         | <span data-ttu-id="7bd01-743">Nome del parametro.</span><span class="sxs-lookup"><span data-stu-id="7bd01-743">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="7bd01-744">**Type**</span><span class="sxs-lookup"><span data-stu-id="7bd01-744">**Type**</span></span>         | <span data-ttu-id="7bd01-745">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-745">No</span></span>          | <span data-ttu-id="7bd01-746">Tipo del parametro.</span><span class="sxs-lookup"><span data-stu-id="7bd01-746">The parameter type.</span></span> <span data-ttu-id="7bd01-747">Un parametro può essere uno qualsiasi dei seguenti tipi (o raccolte di questi tipi):</span><span class="sxs-lookup"><span data-stu-id="7bd01-747">A parameter can be any of the following types (or collections of these types):</span></span> <br/> <span data-ttu-id="7bd01-748">**EdmSimpleType**</span><span class="sxs-lookup"><span data-stu-id="7bd01-748">**EdmSimpleType**</span></span> <br/> <span data-ttu-id="7bd01-749">tipo di entità</span><span class="sxs-lookup"><span data-stu-id="7bd01-749">entity type</span></span> <br/> <span data-ttu-id="7bd01-750">tipo complesso</span><span class="sxs-lookup"><span data-stu-id="7bd01-750">complex type</span></span> <br/> <span data-ttu-id="7bd01-751">tipo di riga</span><span class="sxs-lookup"><span data-stu-id="7bd01-751">row type</span></span> <br/> <span data-ttu-id="7bd01-752">tipo di riferimento</span><span class="sxs-lookup"><span data-stu-id="7bd01-752">reference type</span></span>                             |
| <span data-ttu-id="7bd01-753">**Ammette valori null**</span><span class="sxs-lookup"><span data-stu-id="7bd01-753">**Nullable**</span></span>     | <span data-ttu-id="7bd01-754">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-754">No</span></span>          | <span data-ttu-id="7bd01-755">**True** (valore predefinito) o **False** a seconda del fatto che la proprietà può avere un **null** valore.</span><span class="sxs-lookup"><span data-stu-id="7bd01-755">**True** (the default value) or **False** depending on whether the property can have a **null** value.</span></span>                                                                                                                          |
| <span data-ttu-id="7bd01-756">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="7bd01-756">**DefaultValue**</span></span> | <span data-ttu-id="7bd01-757">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-757">No</span></span>          | <span data-ttu-id="7bd01-758">Valore predefinito della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-758">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="7bd01-759">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="7bd01-759">**MaxLength**</span></span>    | <span data-ttu-id="7bd01-760">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-760">No</span></span>          | <span data-ttu-id="7bd01-761">Lunghezza massima del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-761">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="7bd01-762">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="7bd01-762">**FixedLength**</span></span>  | <span data-ttu-id="7bd01-763">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-763">No</span></span>          | <span data-ttu-id="7bd01-764">**True** oppure **False** a seconda se il valore della proprietà essere archiviato come una stringa di lunghezza fissa.</span><span class="sxs-lookup"><span data-stu-id="7bd01-764">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="7bd01-765">**Precisione**</span><span class="sxs-lookup"><span data-stu-id="7bd01-765">**Precision**</span></span>    | <span data-ttu-id="7bd01-766">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-766">No</span></span>          | <span data-ttu-id="7bd01-767">Precisione del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-767">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="7bd01-768">**Scala**</span><span class="sxs-lookup"><span data-stu-id="7bd01-768">**Scale**</span></span>        | <span data-ttu-id="7bd01-769">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-769">No</span></span>          | <span data-ttu-id="7bd01-770">Scala del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-770">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="7bd01-771">**SRID**</span><span class="sxs-lookup"><span data-stu-id="7bd01-771">**SRID**</span></span>         | <span data-ttu-id="7bd01-772">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-772">No</span></span>          | <span data-ttu-id="7bd01-773">Identificatore di riferimento spaziale del sistema.</span><span class="sxs-lookup"><span data-stu-id="7bd01-773">Spatial System Reference Identifier.</span></span> <span data-ttu-id="7bd01-774">Valido solo per le proprietà di tipi spaziali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-774">Valid only for properties of spatial types.</span></span> <span data-ttu-id="7bd01-775">Per altre informazioni, vedere [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="7bd01-775">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="7bd01-776">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="7bd01-776">**Unicode**</span></span>      | <span data-ttu-id="7bd01-777">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-777">No</span></span>          | <span data-ttu-id="7bd01-778">**True** oppure **False** a seconda se il valore della proprietà essere archiviato come stringa Unicode.</span><span class="sxs-lookup"><span data-stu-id="7bd01-778">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="7bd01-779">**regole di confronto**</span><span class="sxs-lookup"><span data-stu-id="7bd01-779">**Collation**</span></span>    | <span data-ttu-id="7bd01-780">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-780">No</span></span>          | <span data-ttu-id="7bd01-781">Stringa che specifica la sequenza di confronto da utilizzare nell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="7bd01-781">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="7bd01-782">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **parametro** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-782">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="7bd01-783">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-783">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-784">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-784">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="7bd01-785">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-785">Example</span></span>

<span data-ttu-id="7bd01-786">L'esempio seguente mostra una **funzione** elemento che utilizza uno **parametro** elemento figlio per definire un parametro di funzione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-786">The following example shows a **Function** element that uses one **Parameter** child element to define a function parameter.</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
``` 

 

## <a name="principal-element-csdl"></a><span data-ttu-id="7bd01-787">Elemento Principal (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-787">Principal Element (CSDL)</span></span>

<span data-ttu-id="7bd01-788">Il **Principal** conceptual schema definition Language (CSDL) è un elemento figlio all'elemento ReferentialConstraint che definisce l'entità finale di un vincolo referenziale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-788">The **Principal** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element that defines the principal end of a referential constraint.</span></span> <span data-ttu-id="7bd01-789">Oggetto **ReferentialConstraint** elemento definisce la funzionalità che è simile a un vincolo di integrità referenziale in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-789">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="7bd01-790">Nello stesso modo in cui una colonna (o più colonne) da una tabella di database può fare riferimento alla chiave primaria di un'altra tabella, una proprietà (o più proprietà) di un tipo di entità può fare riferimento alla chiave di entità di un altro tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-790">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="7bd01-791">Il tipo di entità cui viene fatto riferimento viene chiamato il *entità finale principale* del vincolo.</span><span class="sxs-lookup"><span data-stu-id="7bd01-791">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="7bd01-792">Il tipo di entità che fa riferimento l'entità finale principale viene chiamato il *entità finale dipendente* del vincolo.</span><span class="sxs-lookup"><span data-stu-id="7bd01-792">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="7bd01-793">**PropertyRef** gli elementi vengono usati per specificare quali chiavi vengono fatto riferimento dall'entità finale dipendente.</span><span class="sxs-lookup"><span data-stu-id="7bd01-793">**PropertyRef** elements are used to specify which keys are referenced by the dependent end.</span></span>

<span data-ttu-id="7bd01-794">Il **Principal** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="7bd01-794">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="7bd01-795">PropertyRef (uno o più elementi)</span><span class="sxs-lookup"><span data-stu-id="7bd01-795">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="7bd01-796">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="7bd01-796">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-797">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-797">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-798">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **Principal** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-798">The table below describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="7bd01-799">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="7bd01-799">Attribute Name</span></span> | <span data-ttu-id="7bd01-800">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="7bd01-800">Is Required</span></span> | <span data-ttu-id="7bd01-801">Valore</span><span class="sxs-lookup"><span data-stu-id="7bd01-801">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="7bd01-802">**Ruolo**</span><span class="sxs-lookup"><span data-stu-id="7bd01-802">**Role**</span></span>       | <span data-ttu-id="7bd01-803">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-803">Yes</span></span>         | <span data-ttu-id="7bd01-804">Nome del tipo di entità in un'entità finale principale dell'associazione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-804">The name of the entity type on the principal end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="7bd01-805">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **Principal** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-805">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="7bd01-806">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-806">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-807">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-807">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="7bd01-808">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-808">Example</span></span>

<span data-ttu-id="7bd01-809">L'esempio seguente mostra una **ReferentialConstraint** elemento che fa parte della definizione delle **PublishedBy** association.</span><span class="sxs-lookup"><span data-stu-id="7bd01-809">The following example shows a **ReferentialConstraint** element that is part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="7bd01-810">Il **Id** proprietà delle **server di pubblicazione** tipo di entità costituisce l'entità finale principale del vincolo referenziale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-810">The **Id** property of the **Publisher** entity type makes up the principal end of the referential constraint.</span></span>

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
 

 

## <a name="property-element-csdl"></a><span data-ttu-id="7bd01-811">Elemento Property (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-811">Property Element (CSDL)</span></span>

<span data-ttu-id="7bd01-812">Il **proprietà** elemento schema conceptual schema definition Language (CSDL) può essere un figlio dell'elemento EntityType, ComplexType (elemento) o l'elemento RowType.</span><span class="sxs-lookup"><span data-stu-id="7bd01-812">The **Property** element in conceptual schema definition language (CSDL) can be a child of the EntityType element, the ComplexType element, or the RowType element.</span></span>

### <a name="entitytype-and-complextype-element-applications"></a><span data-ttu-id="7bd01-813">Applicazione degli elementi EntityType e ComplexType</span><span class="sxs-lookup"><span data-stu-id="7bd01-813">EntityType and ComplexType Element Applications</span></span>

<span data-ttu-id="7bd01-814">**Proprietà** elementi (come figli del **EntityType** oppure **ComplexType** elementi) definiscono la forma e le caratteristiche dei dati che sarà contenuti in un'istanza del tipo di entità o istanza del tipo complesso .</span><span class="sxs-lookup"><span data-stu-id="7bd01-814">**Property** elements (as children of **EntityType** or **ComplexType** elements) define the shape and characteristics of data that an entity type instance or complex type instance will contain.</span></span> <span data-ttu-id="7bd01-815">Le proprietà in un modello concettuale sono analoghe alle proprietà definite su una classe.</span><span class="sxs-lookup"><span data-stu-id="7bd01-815">Properties in a conceptual model are analogous to properties that are defined on a class.</span></span> <span data-ttu-id="7bd01-816">Nello stesso modo in cui le proprietà su una classe definiscono la forma della classe e forniscono informazioni su oggetti, le proprietà in un modello concettuale definiscono la forma di un tipo di entità e forniscono informazioni su istanze del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-816">In the same way that properties on a class define the shape of the class and carry information about objects, properties in a conceptual model define the shape of an entity type and carry information about entity type instances.</span></span>

<span data-ttu-id="7bd01-817">Il **proprietà** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="7bd01-817">The **Property** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="7bd01-818">Elemento documentation (zero o un elemento consentito)</span><span class="sxs-lookup"><span data-stu-id="7bd01-818">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="7bd01-819">Elementi Annotation (zero o più elementi consentiti)</span><span class="sxs-lookup"><span data-stu-id="7bd01-819">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="7bd01-820">I facet seguenti possono essere applicati a un **proprietà** elemento: **Nullable**, **DefaultValue**, **MaxLength**,  **FixedLength**, **precisione**, **scalabilità**, **Unicode**, **delle regole di confronto**,  **ConcurrencyMode**.</span><span class="sxs-lookup"><span data-stu-id="7bd01-820">The following facets can be applied to a **Property** element: **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **Collation**, **ConcurrencyMode**.</span></span> <span data-ttu-id="7bd01-821">I facet sono attributi XML che forniscono informazioni sul modo in cui i valori di proprietà vengono archiviati nell'archivio dati.</span><span class="sxs-lookup"><span data-stu-id="7bd01-821">Facets are XML attributes that provide information about how property values are stored in the data store.</span></span>

> [!NOTE]
> <span data-ttu-id="7bd01-822">I facet possono essere applicati solo alle proprietà di tipo **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="7bd01-822">Facets can only be applied to properties of type **EDMSimpleType**.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-823">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-823">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-824">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **proprietà** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-824">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="7bd01-825">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="7bd01-825">Attribute Name</span></span>                                                         | <span data-ttu-id="7bd01-826">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="7bd01-826">Is Required</span></span> | <span data-ttu-id="7bd01-827">Valore</span><span class="sxs-lookup"><span data-stu-id="7bd01-827">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="7bd01-828">**Name**</span><span class="sxs-lookup"><span data-stu-id="7bd01-828">**Name**</span></span>                                                               | <span data-ttu-id="7bd01-829">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-829">Yes</span></span>         | <span data-ttu-id="7bd01-830">Nome della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-830">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="7bd01-831">**Type**</span><span class="sxs-lookup"><span data-stu-id="7bd01-831">**Type**</span></span>                                                               | <span data-ttu-id="7bd01-832">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-832">Yes</span></span>         | <span data-ttu-id="7bd01-833">Tipo del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-833">The type of the property value.</span></span> <span data-ttu-id="7bd01-834">Il tipo di valore della proprietà deve essere un' **EDMSimpleType** o un tipo complesso (indicato da un nome completo) nell'ambito del modello.</span><span class="sxs-lookup"><span data-stu-id="7bd01-834">The property value type must be an **EDMSimpleType** or a complex type (indicated by a fully-qualified name) that is within scope of the model.</span></span>                                                 |
| <span data-ttu-id="7bd01-835">**Ammette valori null**</span><span class="sxs-lookup"><span data-stu-id="7bd01-835">**Nullable**</span></span>                                                           | <span data-ttu-id="7bd01-836">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-836">No</span></span>          | <span data-ttu-id="7bd01-837">**True** (valore predefinito) o <strong>False</strong> a seconda del fatto che la proprietà può avere un valore null.</span><span class="sxs-lookup"><span data-stu-id="7bd01-837">**True** (the default value) or <strong>False</strong> depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                   |
| <span data-ttu-id="7bd01-838">> Nella versione 1 di CSDL deve avere una proprietà del tipo complesso `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="7bd01-838">> In the CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="7bd01-839">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="7bd01-839">**DefaultValue**</span></span>                                                       | <span data-ttu-id="7bd01-840">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-840">No</span></span>          | <span data-ttu-id="7bd01-841">Valore predefinito della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-841">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="7bd01-842">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="7bd01-842">**MaxLength**</span></span>                                                          | <span data-ttu-id="7bd01-843">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-843">No</span></span>          | <span data-ttu-id="7bd01-844">Lunghezza massima del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-844">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="7bd01-845">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="7bd01-845">**FixedLength**</span></span>                                                        | <span data-ttu-id="7bd01-846">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-846">No</span></span>          | <span data-ttu-id="7bd01-847">**True** oppure **False** a seconda se il valore della proprietà essere archiviato come una stringa di lunghezza fissa.</span><span class="sxs-lookup"><span data-stu-id="7bd01-847">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="7bd01-848">**Precisione**</span><span class="sxs-lookup"><span data-stu-id="7bd01-848">**Precision**</span></span>                                                          | <span data-ttu-id="7bd01-849">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-849">No</span></span>          | <span data-ttu-id="7bd01-850">Precisione del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-850">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="7bd01-851">**Scala**</span><span class="sxs-lookup"><span data-stu-id="7bd01-851">**Scale**</span></span>                                                              | <span data-ttu-id="7bd01-852">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-852">No</span></span>          | <span data-ttu-id="7bd01-853">Scala del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-853">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="7bd01-854">**SRID**</span><span class="sxs-lookup"><span data-stu-id="7bd01-854">**SRID**</span></span>                                                               | <span data-ttu-id="7bd01-855">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-855">No</span></span>          | <span data-ttu-id="7bd01-856">Identificatore di riferimento spaziale del sistema.</span><span class="sxs-lookup"><span data-stu-id="7bd01-856">Spatial System Reference Identifier.</span></span> <span data-ttu-id="7bd01-857">Valido solo per le proprietà di tipi spaziali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-857">Valid only for properties of spatial types.</span></span> <span data-ttu-id="7bd01-858">Per altre informazioni, vedere [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="7bd01-858">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="7bd01-859">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="7bd01-859">**Unicode**</span></span>                                                            | <span data-ttu-id="7bd01-860">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-860">No</span></span>          | <span data-ttu-id="7bd01-861">**True** oppure **False** a seconda se il valore della proprietà essere archiviato come stringa Unicode.</span><span class="sxs-lookup"><span data-stu-id="7bd01-861">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="7bd01-862">**regole di confronto**</span><span class="sxs-lookup"><span data-stu-id="7bd01-862">**Collation**</span></span>                                                          | <span data-ttu-id="7bd01-863">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-863">No</span></span>          | <span data-ttu-id="7bd01-864">Stringa che specifica la sequenza di confronto da utilizzare nell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="7bd01-864">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="7bd01-865">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="7bd01-865">**ConcurrencyMode**</span></span>                                                    | <span data-ttu-id="7bd01-866">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-866">No</span></span>          | <span data-ttu-id="7bd01-867">**None** (valore predefinito) o **Fixed**.</span><span class="sxs-lookup"><span data-stu-id="7bd01-867">**None** (the default value) or **Fixed**.</span></span> <span data-ttu-id="7bd01-868">Se il valore è impostato su **Fixed**, verrà utilizzato il valore della proprietà nei controlli di concorrenza ottimistica.</span><span class="sxs-lookup"><span data-stu-id="7bd01-868">If the value is set to **Fixed**, the property value will be used in optimistic concurrency checks.</span></span>                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="7bd01-869">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **proprietà** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-869">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="7bd01-870">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-870">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-871">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-871">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="7bd01-872">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-872">Example</span></span>

<span data-ttu-id="7bd01-873">L'esempio seguente mostra un' **EntityType** elemento con tre **proprietà** elementi:</span><span class="sxs-lookup"><span data-stu-id="7bd01-873">The following example shows an **EntityType** element with three **Property** elements:</span></span>

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
 

<span data-ttu-id="7bd01-874">L'esempio seguente mostra una **ComplexType** elemento con cinque **proprietà** elementi:</span><span class="sxs-lookup"><span data-stu-id="7bd01-874">The following example shows a **ComplexType** element with five **Property** elements:</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a><span data-ttu-id="7bd01-875">Applicazione dell'elemento RowType</span><span class="sxs-lookup"><span data-stu-id="7bd01-875">RowType Element Application</span></span>

<span data-ttu-id="7bd01-876">**Proprietà** elementi (come figli di un **RowType** elemento) definiscono la forma e le caratteristiche dei dati che possono essere passati a o restituiti da una funzione definita dal modello.</span><span class="sxs-lookup"><span data-stu-id="7bd01-876">**Property** elements (as the children of a **RowType** element) define the shape and characteristics of data that can be passed to or returned from a model-defined function.</span></span>  

<span data-ttu-id="7bd01-877">Il **proprietà** elemento può includere esattamente uno degli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="7bd01-877">The **Property** element can have exactly one of the following child elements:</span></span>

-   <span data-ttu-id="7bd01-878">CollectionType</span><span class="sxs-lookup"><span data-stu-id="7bd01-878">CollectionType</span></span>
-   <span data-ttu-id="7bd01-879">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="7bd01-879">ReferenceType</span></span>
-   <span data-ttu-id="7bd01-880">RowType</span><span class="sxs-lookup"><span data-stu-id="7bd01-880">RowType</span></span>

<span data-ttu-id="7bd01-881">Il **proprietà** elemento può avere qualsiasi numero figlio annotazione di elementi.</span><span class="sxs-lookup"><span data-stu-id="7bd01-881">The **Property** element can have any number child annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="7bd01-882">Elementi di annotazione sono consentiti solo nella versione 2 CSDL e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="7bd01-882">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-883">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-883">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-884">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **proprietà** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-884">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="7bd01-885">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="7bd01-885">Attribute Name</span></span>                                                     | <span data-ttu-id="7bd01-886">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="7bd01-886">Is Required</span></span> | <span data-ttu-id="7bd01-887">Valore</span><span class="sxs-lookup"><span data-stu-id="7bd01-887">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="7bd01-888">**Name**</span><span class="sxs-lookup"><span data-stu-id="7bd01-888">**Name**</span></span>                                                           | <span data-ttu-id="7bd01-889">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-889">Yes</span></span>         | <span data-ttu-id="7bd01-890">Nome della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-890">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="7bd01-891">**Type**</span><span class="sxs-lookup"><span data-stu-id="7bd01-891">**Type**</span></span>                                                           | <span data-ttu-id="7bd01-892">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-892">Yes</span></span>         | <span data-ttu-id="7bd01-893">Tipo del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-893">The type of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="7bd01-894">**Ammette valori null**</span><span class="sxs-lookup"><span data-stu-id="7bd01-894">**Nullable**</span></span>                                                       | <span data-ttu-id="7bd01-895">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-895">No</span></span>          | <span data-ttu-id="7bd01-896">**True** (valore predefinito) o **False** a seconda del fatto che la proprietà può avere un valore null.</span><span class="sxs-lookup"><span data-stu-id="7bd01-896">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="7bd01-897">> Nella versione 1 CSDL deve avere una proprietà del tipo complesso `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="7bd01-897">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="7bd01-898">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="7bd01-898">**DefaultValue**</span></span>                                                   | <span data-ttu-id="7bd01-899">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-899">No</span></span>          | <span data-ttu-id="7bd01-900">Valore predefinito della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-900">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="7bd01-901">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="7bd01-901">**MaxLength**</span></span>                                                      | <span data-ttu-id="7bd01-902">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-902">No</span></span>          | <span data-ttu-id="7bd01-903">Lunghezza massima del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-903">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="7bd01-904">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="7bd01-904">**FixedLength**</span></span>                                                    | <span data-ttu-id="7bd01-905">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-905">No</span></span>          | <span data-ttu-id="7bd01-906">**True** oppure **False** a seconda se il valore della proprietà essere archiviato come una stringa di lunghezza fissa.</span><span class="sxs-lookup"><span data-stu-id="7bd01-906">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="7bd01-907">**Precisione**</span><span class="sxs-lookup"><span data-stu-id="7bd01-907">**Precision**</span></span>                                                      | <span data-ttu-id="7bd01-908">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-908">No</span></span>          | <span data-ttu-id="7bd01-909">Precisione del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-909">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="7bd01-910">**Scala**</span><span class="sxs-lookup"><span data-stu-id="7bd01-910">**Scale**</span></span>                                                          | <span data-ttu-id="7bd01-911">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-911">No</span></span>          | <span data-ttu-id="7bd01-912">Scala del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-912">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="7bd01-913">**SRID**</span><span class="sxs-lookup"><span data-stu-id="7bd01-913">**SRID**</span></span>                                                           | <span data-ttu-id="7bd01-914">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-914">No</span></span>          | <span data-ttu-id="7bd01-915">Identificatore di riferimento spaziale del sistema.</span><span class="sxs-lookup"><span data-stu-id="7bd01-915">Spatial System Reference Identifier.</span></span> <span data-ttu-id="7bd01-916">Valido solo per le proprietà di tipi spaziali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-916">Valid only for properties of spatial types.</span></span> <span data-ttu-id="7bd01-917">Per altre informazioni, vedere [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="7bd01-917">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="7bd01-918">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="7bd01-918">**Unicode**</span></span>                                                        | <span data-ttu-id="7bd01-919">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-919">No</span></span>          | <span data-ttu-id="7bd01-920">**True** oppure **False** a seconda se il valore della proprietà essere archiviato come stringa Unicode.</span><span class="sxs-lookup"><span data-stu-id="7bd01-920">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="7bd01-921">**regole di confronto**</span><span class="sxs-lookup"><span data-stu-id="7bd01-921">**Collation**</span></span>                                                      | <span data-ttu-id="7bd01-922">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-922">No</span></span>          | <span data-ttu-id="7bd01-923">Stringa che specifica la sequenza di confronto da utilizzare nell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="7bd01-923">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="7bd01-924">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **proprietà** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-924">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="7bd01-925">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-925">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-926">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-926">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="7bd01-927">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-927">Example</span></span>

<span data-ttu-id="7bd01-928">L'esempio seguente illustra **proprietà** elementi usati per definire la forma del tipo restituito di una funzione definita dal modello.</span><span class="sxs-lookup"><span data-stu-id="7bd01-928">The following example shows **Property** elements used to define the shape of the return type of a model-defined function.</span></span>

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
 

 

## <a name="propertyref-element-csdl"></a><span data-ttu-id="7bd01-929">Elemento PropertyRef (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-929">PropertyRef Element (CSDL)</span></span>

<span data-ttu-id="7bd01-930">Il **PropertyRef** elemento schema conceptual schema definition Language (CSDL) fa riferimento a una proprietà di un tipo di entità per indicare che la proprietà eseguirà uno dei seguenti ruoli:</span><span class="sxs-lookup"><span data-stu-id="7bd01-930">The **PropertyRef** element in conceptual schema definition language (CSDL) references a property of an entity type to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="7bd01-931">Parte della chiave di entità (una proprietà o un set di proprietà di un tipo di entità che determinano l'identità).</span><span class="sxs-lookup"><span data-stu-id="7bd01-931">Part of the entity's key (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="7bd01-932">Uno o più **PropertyRef** elementi possono essere utilizzati per definire una chiave di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-932">One or more **PropertyRef** elements can be used to define an entity key.</span></span>
-   <span data-ttu-id="7bd01-933">L'entità finale dipendente o principale di un vincolo referenziale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-933">The dependent or principal end of a referential constraint.</span></span>

<span data-ttu-id="7bd01-934">Il **PropertyRef** elemento può avere solo elementi di annotazione (zero o più) come elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="7bd01-934">The **PropertyRef** element can only have annotation elements (zero or more) as child elements.</span></span>

> [!NOTE]
> <span data-ttu-id="7bd01-935">Elementi di annotazione sono consentiti solo nella versione 2 CSDL e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="7bd01-935">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-936">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-936">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-937">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **PropertyRef** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-937">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="7bd01-938">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="7bd01-938">Attribute Name</span></span> | <span data-ttu-id="7bd01-939">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="7bd01-939">Is Required</span></span> | <span data-ttu-id="7bd01-940">Valore</span><span class="sxs-lookup"><span data-stu-id="7bd01-940">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="7bd01-941">**Name**</span><span class="sxs-lookup"><span data-stu-id="7bd01-941">**Name**</span></span>       | <span data-ttu-id="7bd01-942">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-942">Yes</span></span>         | <span data-ttu-id="7bd01-943">Nome della proprietà alla quale viene fatto riferimento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-943">The name of the referenced property.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="7bd01-944">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **PropertyRef** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-944">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="7bd01-945">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-945">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-946">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-946">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="7bd01-947">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-947">Example</span></span>

<span data-ttu-id="7bd01-948">L'esempio seguente definisce un tipo di entità (**libro**).</span><span class="sxs-lookup"><span data-stu-id="7bd01-948">The example below defines an entity type (**Book**).</span></span> <span data-ttu-id="7bd01-949">La chiave di entità viene definita facendo il **ISBN** proprietà del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-949">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="7bd01-950">Nell'esempio seguente, due **PropertyRef** gli elementi vengono usati per indicare che due proprietà (**Id** e **PublisherId**) sono le entità finali principale e dipendente di un referenziale vincolo.</span><span class="sxs-lookup"><span data-stu-id="7bd01-950">In the next example, two **PropertyRef** elements are used to indicate that two properties (**Id** and **PublisherId**) are the principal and dependent ends of a referential constraint.</span></span>

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
 

 

## <a name="referencetype-element-csdl"></a><span data-ttu-id="7bd01-951">Elemento ReferenceType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-951">ReferenceType Element (CSDL)</span></span>

<span data-ttu-id="7bd01-952">Il **ReferenceType** elemento schema conceptual schema definition Language (CSDL) specifica un riferimento a un tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-952">The **ReferenceType** element in conceptual schema definition language (CSDL) specifies a reference to an entity type.</span></span> <span data-ttu-id="7bd01-953">Il **ReferenceType** elemento può essere un elemento figlio degli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="7bd01-953">The **ReferenceType** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="7bd01-954">ReturnType (funzione)</span><span class="sxs-lookup"><span data-stu-id="7bd01-954">ReturnType (Function)</span></span>
-   <span data-ttu-id="7bd01-955">Parametro</span><span class="sxs-lookup"><span data-stu-id="7bd01-955">Parameter</span></span>
-   <span data-ttu-id="7bd01-956">CollectionType</span><span class="sxs-lookup"><span data-stu-id="7bd01-956">CollectionType</span></span>

<span data-ttu-id="7bd01-957">Il **ReferenceType** elemento viene usato quando si definisce un parametro o tipo restituito per una funzione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-957">The **ReferenceType** element is used when defining a parameter or return type for a function.</span></span>

<span data-ttu-id="7bd01-958">Oggetto **ReferenceType** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="7bd01-958">A **ReferenceType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="7bd01-959">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="7bd01-959">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="7bd01-960">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="7bd01-960">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-961">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-961">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-962">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **ReferenceType** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-962">The table below describes the attributes that can be applied to the **ReferenceType** element.</span></span>

| <span data-ttu-id="7bd01-963">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="7bd01-963">Attribute Name</span></span> | <span data-ttu-id="7bd01-964">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="7bd01-964">Is Required</span></span> | <span data-ttu-id="7bd01-965">Valore</span><span class="sxs-lookup"><span data-stu-id="7bd01-965">Value</span></span>                                         |
|:---------------|:------------|:----------------------------------------------|
| <span data-ttu-id="7bd01-966">**Type**</span><span class="sxs-lookup"><span data-stu-id="7bd01-966">**Type**</span></span>       | <span data-ttu-id="7bd01-967">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-967">Yes</span></span>         | <span data-ttu-id="7bd01-968">Nome del tipo di entità a cui viene fatto riferimento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-968">The name of the entity type being referenced.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="7bd01-969">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **ReferenceType** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-969">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferenceType** element.</span></span> <span data-ttu-id="7bd01-970">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-970">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-971">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-971">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="7bd01-972">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-972">Example</span></span>

<span data-ttu-id="7bd01-973">Nell'esempio seguente il **ReferenceType** elemento utilizzato come elemento figlio di un **parametro** elemento in una funzione definita dal modello che accetta un riferimento a una **persona** entità tipo:</span><span class="sxs-lookup"><span data-stu-id="7bd01-973">The following example shows the **ReferenceType** element used as a child of a **Parameter** element in a model-defined function that accepts a reference to a **Person** entity type:</span></span>

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
 

<span data-ttu-id="7bd01-974">Nell'esempio seguente il **ReferenceType** elemento utilizzato come elemento figlio di un **ReturnType** elemento (funzione) in una funzione definita dal modello che restituisce un riferimento a un **persona**tipo di entità:</span><span class="sxs-lookup"><span data-stu-id="7bd01-974">The following example shows the **ReferenceType** element used as a child of a **ReturnType** (Function) element in a model-defined function that returns a reference to a **Person** entity type:</span></span>

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
 

 

## <a name="referentialconstraint-element-csdl"></a><span data-ttu-id="7bd01-975">Elemento ReferentialConstraint (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-975">ReferentialConstraint Element (CSDL)</span></span>

<span data-ttu-id="7bd01-976">Oggetto **ReferentialConstraint** elemento schema conceptual schema definition Language (CSDL) definisce la funzionalità che è simile a un vincolo di integrità referenziale in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-976">A **ReferentialConstraint** element in conceptual schema definition language (CSDL) defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="7bd01-977">Nello stesso modo in cui una colonna (o più colonne) da una tabella di database può fare riferimento alla chiave primaria di un'altra tabella, una proprietà (o più proprietà) di un tipo di entità può fare riferimento alla chiave di entità di un altro tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-977">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="7bd01-978">Il tipo di entità cui viene fatto riferimento viene chiamato il *entità finale principale* del vincolo.</span><span class="sxs-lookup"><span data-stu-id="7bd01-978">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="7bd01-979">Il tipo di entità che fa riferimento l'entità finale principale viene chiamato il *entità finale dipendente* del vincolo.</span><span class="sxs-lookup"><span data-stu-id="7bd01-979">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span>

<span data-ttu-id="7bd01-980">Se una chiave esterna che viene esposto in un tipo di entità fa riferimento a una proprietà in un altro tipo di entità, il **ReferentialConstraint** elemento definisce un'associazione tra i due tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-980">If a foreign key that is exposed on one entity type references a property on another entity type, the **ReferentialConstraint** element defines an association between the two entity types.</span></span> <span data-ttu-id="7bd01-981">Poiché il **ReferentialConstraint** elemento fornisce informazioni sul modo in cui due tipi di entità sono correlate, nessun elemento AssociationSetMapping corrispondente è necessario lo schema mapping specification language (MSL).</span><span class="sxs-lookup"><span data-stu-id="7bd01-981">Because the **ReferentialConstraint** element provides information about how two entity types are related, no corresponding AssociationSetMapping element is necessary in the mapping specification language (MSL).</span></span> <span data-ttu-id="7bd01-982">Un'associazione tra due tipi di entità che non dispongono di chiavi esterne esposte deve disporre di un oggetto corrispondente **AssociationSetMapping** elemento per eseguire il mapping di informazioni sull'associazione all'origine dati.</span><span class="sxs-lookup"><span data-stu-id="7bd01-982">An association between two entity types that do not have foreign keys exposed must have a corresponding **AssociationSetMapping** element in order to map association information to the data source.</span></span>

<span data-ttu-id="7bd01-983">Se una chiave esterna non viene esposto in un tipo di entità, il **ReferentialConstraint** elemento può definire solo un vincolo primary key-a-primary key tra il tipo di entità e un altro tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-983">If a foreign key is not exposed on an entity type, the **ReferentialConstraint** element can only define a primary key-to-primary key constraint between the entity type and another entity type.</span></span>

<span data-ttu-id="7bd01-984">Oggetto **ReferentialConstraint** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="7bd01-984">A **ReferentialConstraint** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="7bd01-985">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="7bd01-985">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="7bd01-986">Entità (esattamente un elemento)</span><span class="sxs-lookup"><span data-stu-id="7bd01-986">Principal (exactly one element)</span></span>
-   <span data-ttu-id="7bd01-987">Dipendente (esattamente un elemento)</span><span class="sxs-lookup"><span data-stu-id="7bd01-987">Dependent (exactly one element)</span></span>
-   <span data-ttu-id="7bd01-988">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="7bd01-988">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-989">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-989">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-990">Il **ReferentialConstraint** elemento può avere qualsiasi numero di attributi di annotazione (attributi XML personalizzati).</span><span class="sxs-lookup"><span data-stu-id="7bd01-990">The **ReferentialConstraint** element can have any number of annotation attributes (custom XML attributes).</span></span> <span data-ttu-id="7bd01-991">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-991">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-992">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-992">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="7bd01-993">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-993">Example</span></span>

<span data-ttu-id="7bd01-994">L'esempio seguente mostra una **ReferentialConstraint** elemento utilizzato come parte della definizione del **PublishedBy** association.</span><span class="sxs-lookup"><span data-stu-id="7bd01-994">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span>

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
 

 

## <a name="returntype-function-element-csdl"></a><span data-ttu-id="7bd01-995">Elemento ReturnType (funzione) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-995">ReturnType (Function) Element (CSDL)</span></span>

<span data-ttu-id="7bd01-996">Il **ReturnType** (funzione) elemento schema conceptual schema definition Language (CSDL) specifica il tipo restituito per una funzione che viene definito in un elemento di funzione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-996">The **ReturnType** (Function) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a Function element.</span></span> <span data-ttu-id="7bd01-997">Una funzione di tipo restituito può essere specificato anche con un **ReturnType** attributo.</span><span class="sxs-lookup"><span data-stu-id="7bd01-997">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="7bd01-998">Tipi restituiti possono essere qualsiasi **EdmSimpleType**, tipo di entità, tipo complesso, tipo di riga, tipo di riferimento o una raccolta di uno di questi tipi.</span><span class="sxs-lookup"><span data-stu-id="7bd01-998">Return types can be any **EdmSimpleType**, entity type, complex type, row type, ref type, or a collection of one of these types.</span></span>

<span data-ttu-id="7bd01-999">Il tipo restituito di una funzione può essere specificato mediante il **tipo** attributo del **ReturnType** elemento (funzione), o con uno degli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="7bd01-999">The return type of a function can be specified with either the **Type** attribute of the **ReturnType** (Function) element, or with one of the following child elements:</span></span>

-   <span data-ttu-id="7bd01-1000">CollectionType</span><span class="sxs-lookup"><span data-stu-id="7bd01-1000">CollectionType</span></span>
-   <span data-ttu-id="7bd01-1001">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="7bd01-1001">ReferenceType</span></span>
-   <span data-ttu-id="7bd01-1002">RowType</span><span class="sxs-lookup"><span data-stu-id="7bd01-1002">RowType</span></span>

> [!NOTE]
> <span data-ttu-id="7bd01-1003">Un modello non eseguirà la convalida se si specifica un restituito dalla funzione tipo con entrambe le **tipo** attributo del **ReturnType** (Function) (elemento) e uno degli elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1003">A model will not validate if you specify a function return type with both the **Type** attribute of the **ReturnType** (Function) element and one of the child elements.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-1004">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-1004">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-1005">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **ReturnType** (Function) (elemento).</span><span class="sxs-lookup"><span data-stu-id="7bd01-1005">The following table describes the attributes that can be applied to the **ReturnType** (Function) element.</span></span>

| <span data-ttu-id="7bd01-1006">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="7bd01-1006">Attribute Name</span></span> | <span data-ttu-id="7bd01-1007">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="7bd01-1007">Is Required</span></span> | <span data-ttu-id="7bd01-1008">Valore</span><span class="sxs-lookup"><span data-stu-id="7bd01-1008">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="7bd01-1009">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1009">**ReturnType**</span></span> | <span data-ttu-id="7bd01-1010">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-1010">No</span></span>          | <span data-ttu-id="7bd01-1011">Tipo restituito dalla funzione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1011">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="7bd01-1012">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **ReturnType** (Function) (elemento).</span><span class="sxs-lookup"><span data-stu-id="7bd01-1012">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (Function) element.</span></span> <span data-ttu-id="7bd01-1013">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1013">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-1014">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1014">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="7bd01-1015">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-1015">Example</span></span>

<span data-ttu-id="7bd01-1016">L'esempio seguente usa un' **funzione** elemento per definire una funzione che restituisce il numero di anni in un libro è stato in stampa.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1016">The following example uses a **Function** element to define a function that returns the number of years a book has been in print.</span></span> <span data-ttu-id="7bd01-1017">Si noti che il tipo restituito è specificato per il **tipo** attributo di un **ReturnType** (Function) (elemento).</span><span class="sxs-lookup"><span data-stu-id="7bd01-1017">Note that the return type is specified by the **Type** attribute of a **ReturnType** (Function) element.</span></span>

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a><span data-ttu-id="7bd01-1018">Elemento ReturnType (FunctionImport) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-1018">ReturnType (FunctionImport) Element (CSDL)</span></span>

<span data-ttu-id="7bd01-1019">Il **ReturnType** (FunctionImport) elemento schema conceptual schema definition Language (CSDL) specifica il tipo restituito per una funzione che viene definito in un elemento FunctionImport.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1019">The **ReturnType** (FunctionImport) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a FunctionImport element.</span></span> <span data-ttu-id="7bd01-1020">Una funzione di tipo restituito può essere specificato anche con un **ReturnType** attributo.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1020">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="7bd01-1021">Tipi restituiti possono essere qualsiasi raccolta di tipo di entità, tipo complesso, oppure **EdmSimpleType**,</span><span class="sxs-lookup"><span data-stu-id="7bd01-1021">Return types can be any collection of entity type, complex type,or **EdmSimpleType**,</span></span>

<span data-ttu-id="7bd01-1022">Viene specificato il tipo restituito di una funzione con il **tipo** attributo il **ReturnType** elemento (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="7bd01-1022">The return type of a function is specified with the **Type** attribute of the **ReturnType** (FunctionImport) element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-1023">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-1023">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-1024">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **ReturnType** elemento (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="7bd01-1024">The following table describes the attributes that can be applied to the **ReturnType** (FunctionImport) element.</span></span>

| <span data-ttu-id="7bd01-1025">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="7bd01-1025">Attribute Name</span></span> | <span data-ttu-id="7bd01-1026">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="7bd01-1026">Is Required</span></span> | <span data-ttu-id="7bd01-1027">Valore</span><span class="sxs-lookup"><span data-stu-id="7bd01-1027">Value</span></span>                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="7bd01-1028">**Type**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1028">**Type**</span></span>       | <span data-ttu-id="7bd01-1029">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-1029">No</span></span>          | <span data-ttu-id="7bd01-1030">Tipo restituito dalla funzione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1030">The type that the function returns.</span></span> <span data-ttu-id="7bd01-1031">Il valore deve essere una raccolta di ComplexType, EntityType o EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1031">The value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>                                                                                      |
| <span data-ttu-id="7bd01-1032">**Elemento EntitySet**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1032">**EntitySet**</span></span>  | <span data-ttu-id="7bd01-1033">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-1033">No</span></span>          | <span data-ttu-id="7bd01-1034">Se la funzione restituisce una raccolta di entità dei tipi, il valore della **EntitySet** deve essere il set di entità a cui appartiene la raccolta.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1034">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="7bd01-1035">In caso contrario, il **EntitySet** attributo non deve essere usato.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1035">Otherwise, the **EntitySet** attribute must not be used.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="7bd01-1036">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **ReturnType** elemento (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="7bd01-1036">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (FunctionImport) element.</span></span> <span data-ttu-id="7bd01-1037">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1037">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-1038">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1038">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="7bd01-1039">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-1039">Example</span></span>

<span data-ttu-id="7bd01-1040">L'esempio seguente usa un' **FunctionImport** che restituisce libri e i server di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1040">The following example uses a **FunctionImport** that returns books and publishers.</span></span> <span data-ttu-id="7bd01-1041">Si noti che la funzione restituisce due set di risultati e quindi due **ReturnType** vengono specificati gli elementi (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="7bd01-1041">Note that the function returns two result sets and therefore two **ReturnType** (FunctionImport) elements are specified.</span></span>

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a><span data-ttu-id="7bd01-1042">Elemento RowType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-1042">RowType Element (CSDL)</span></span>

<span data-ttu-id="7bd01-1043">Oggetto **RowType** elemento schema conceptual schema definition Language (CSDL) definisce una struttura senza nome come parametro o tipo restituito per una funzione definita nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1043">A **RowType** element in conceptual schema definition language (CSDL) defines an unnamed structure as a parameter or return type for a function defined in the conceptual model.</span></span>

<span data-ttu-id="7bd01-1044">Oggetto **RowType** elemento può essere l'elemento figlio degli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="7bd01-1044">A **RowType** element can be the child of the following elements:</span></span>

-   <span data-ttu-id="7bd01-1045">CollectionType</span><span class="sxs-lookup"><span data-stu-id="7bd01-1045">CollectionType</span></span>
-   <span data-ttu-id="7bd01-1046">Parametro</span><span class="sxs-lookup"><span data-stu-id="7bd01-1046">Parameter</span></span>
-   <span data-ttu-id="7bd01-1047">ReturnType (funzione)</span><span class="sxs-lookup"><span data-stu-id="7bd01-1047">ReturnType (Function)</span></span>

<span data-ttu-id="7bd01-1048">Oggetto **RowType** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="7bd01-1048">A **RowType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="7bd01-1049">Proprietà (una o più)</span><span class="sxs-lookup"><span data-stu-id="7bd01-1049">Property (one or more)</span></span>
-   <span data-ttu-id="7bd01-1050">Elementi Annotation (zero o più)</span><span class="sxs-lookup"><span data-stu-id="7bd01-1050">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-1051">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-1051">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-1052">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **RowType** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1052">Any number of annotation attributes (custom XML attributes) may be applied to the **RowType** element.</span></span> <span data-ttu-id="7bd01-1053">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1053">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-1054">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1054">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="7bd01-1055">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-1055">Example</span></span>

<span data-ttu-id="7bd01-1056">L'esempio seguente illustra una funzione definita dal modello che usa un' **CollectionType** elemento per specificare che la funzione restituisce una raccolta di righe (come specificato nella **RowType** elemento).</span><span class="sxs-lookup"><span data-stu-id="7bd01-1056">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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

## <a name="schema-element-csdl"></a><span data-ttu-id="7bd01-1057">Elemento Schema (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-1057">Schema Element (CSDL)</span></span>

<span data-ttu-id="7bd01-1058">Il **Schema** elemento è l'elemento radice di una definizione del modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1058">The **Schema** element is the root element of a conceptual model definition.</span></span> <span data-ttu-id="7bd01-1059">Contiene definizioni per gli oggetti, le funzioni e i contenitori che costituiscono un modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1059">It contains definitions for the objects, functions, and containers that make up a conceptual model.</span></span>

<span data-ttu-id="7bd01-1060">Il **Schema** elemento può contenere zero o più degli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="7bd01-1060">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="7bd01-1061">Using</span><span class="sxs-lookup"><span data-stu-id="7bd01-1061">Using</span></span>
-   <span data-ttu-id="7bd01-1062">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="7bd01-1062">EntityContainer</span></span>
-   <span data-ttu-id="7bd01-1063">EntityType</span><span class="sxs-lookup"><span data-stu-id="7bd01-1063">EntityType</span></span>
-   <span data-ttu-id="7bd01-1064">EnumType</span><span class="sxs-lookup"><span data-stu-id="7bd01-1064">EnumType</span></span>
-   <span data-ttu-id="7bd01-1065">Associazione</span><span class="sxs-lookup"><span data-stu-id="7bd01-1065">Association</span></span>
-   <span data-ttu-id="7bd01-1066">ComplexType</span><span class="sxs-lookup"><span data-stu-id="7bd01-1066">ComplexType</span></span>
-   <span data-ttu-id="7bd01-1067">Funzione</span><span class="sxs-lookup"><span data-stu-id="7bd01-1067">Function</span></span>

<span data-ttu-id="7bd01-1068">Oggetto **Schema** elemento può contenere zero o un elemento di annotazione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1068">A **Schema** element may contain zero or one Annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="7bd01-1069">Il **funzione** elemento e gli elementi di annotazione sono consentiti solo nella versione 2 CSDL e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1069">The **Function** element and annotation elements are only allowed in CSDL v2 and later.</span></span>

 

<span data-ttu-id="7bd01-1070">Il **Schema** elemento utilizza le **Namespace** attributo per definire lo spazio dei nomi per il tipo di entità, tipo complesso e gli oggetti di associazione in un modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1070">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type, complex type, and association objects in a conceptual model.</span></span> <span data-ttu-id="7bd01-1071">All'interno di uno spazio dei nomi due oggetti non possono avere lo stesso nome.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1071">Within a namespace, no two objects can have the same name.</span></span> <span data-ttu-id="7bd01-1072">Gli spazi dei nomi possono estendersi a più **Schema** elementi e più file con estensione CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1072">Namespaces can span multiple **Schema** elements and multiple .csdl files.</span></span>

<span data-ttu-id="7bd01-1073">Uno spazio dei nomi del modello concettuale è diverso dallo spazio dei nomi XML del **Schema** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1073">A conceptual model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="7bd01-1074">Uno spazio dei nomi del modello concettuale (come definito per il **Namespace** attributi) è un contenitore logico per tipi di entità, tipi complessi e tipi di associazione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1074">A conceptual model namespace (as defined by the **Namespace** attribute) is a logical container for entity types, complex types, and association types.</span></span> <span data-ttu-id="7bd01-1075">Lo spazio dei nomi XML (indicato dal **xmlns** attributo) di un **Schema** elemento è lo spazio dei nomi predefinito per gli elementi figlio e attributi del **Schema** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1075">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="7bd01-1076">Spazi dei nomi XML nel formato http://schemas.microsoft.com/ado/YYYY/MM/edm (dove YYYY e MM rappresentano un anno e mese rispettivamente) sono riservati a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1076">XML namespaces of the form http://schemas.microsoft.com/ado/YYYY/MM/edm (where YYYY and MM represent a year and month respectively) are reserved for CSDL.</span></span> <span data-ttu-id="7bd01-1077">Gli elementi e gli attributi personalizzati non possono essere in spazi dei nomi che hanno questo form.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1077">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-1078">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-1078">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-1079">Nella tabella seguente vengono descritti gli attributi possono essere applicati per il **Schema** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1079">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="7bd01-1080">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="7bd01-1080">Attribute Name</span></span> | <span data-ttu-id="7bd01-1081">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="7bd01-1081">Is Required</span></span> | <span data-ttu-id="7bd01-1082">Valore</span><span class="sxs-lookup"><span data-stu-id="7bd01-1082">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="7bd01-1083">**Spazio dei nomi**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1083">**Namespace**</span></span>  | <span data-ttu-id="7bd01-1084">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-1084">Yes</span></span>         | <span data-ttu-id="7bd01-1085">Spazio dei nomi del modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1085">The namespace of the conceptual model.</span></span> <span data-ttu-id="7bd01-1086">Il valore della **Namespace** viene usato in modo da formare il nome completo di un tipo.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1086">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="7bd01-1087">Ad esempio, se un **EntityType** denominata *cliente* trova nello spazio dei nomi si, specificare il nome completo del **EntityType** è Simpleexamplemodel.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1087">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace, then the fully qualified name of the **EntityType** is SimpleExampleModel.Customer.</span></span> <br/> <span data-ttu-id="7bd01-1088">Le stringhe seguenti non possono essere usate come valore per il **Namespace** attributo: **System**, **temporaneo**, oppure **Edm**.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1088">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="7bd01-1089">Il valore per il **Namespace** attributo non può essere identico al valore per il **Namespace** attributo nell'elemento di Schema SSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1089">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the SSDL Schema element.</span></span> |
| <span data-ttu-id="7bd01-1090">**Alias**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1090">**Alias**</span></span>      | <span data-ttu-id="7bd01-1091">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-1091">No</span></span>          | <span data-ttu-id="7bd01-1092">Identificatore utilizzato al posto del nome dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1092">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="7bd01-1093">Ad esempio, se un **EntityType** denominata *cliente* è lo spazio dei nomi si e il valore del **Alias** attributo è *Model*, è possibile utilizzare Model. Customer come nome completo del **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1093">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace and the value of the **Alias** attribute is *Model*, then you can use Model.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="7bd01-1094">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **Schema** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1094">Any number of annotation attributes (custom XML attributes) may be applied to the **Schema** element.</span></span> <span data-ttu-id="7bd01-1095">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1095">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-1096">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1096">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="7bd01-1097">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-1097">Example</span></span>

<span data-ttu-id="7bd01-1098">L'esempio seguente mostra una **Schema** elemento che contiene un' **EntityContainer** elemento, due **EntityType** elementi e uno **associazione** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1098">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

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
 

 

## <a name="typeref-element-csdl"></a><span data-ttu-id="7bd01-1099">Elemento TypeRef (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-1099">TypeRef Element (CSDL)</span></span>

<span data-ttu-id="7bd01-1100">Il **TypeRef** elemento schema conceptual schema definition Language (CSDL) fornisce un riferimento a un tipo con nome esistente.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1100">The **TypeRef** element in conceptual schema definition language (CSDL) provides a reference to an existing named type.</span></span> <span data-ttu-id="7bd01-1101">Il **TypeRef** elemento può essere un figlio dell'elemento CollectionType, che consente di specificare che una funzione dispone di una raccolta come un parametro o tipo restituito.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1101">The **TypeRef** element can be a child of the CollectionType element, which is used to specify that a function has a collection as a parameter or return type.</span></span>

<span data-ttu-id="7bd01-1102">Oggetto **TypeRef** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="7bd01-1102">A **TypeRef** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="7bd01-1103">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="7bd01-1103">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="7bd01-1104">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="7bd01-1104">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-1105">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-1105">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-1106">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **TypeRef** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1106">The following table describes the attributes that can be applied to the **TypeRef** element.</span></span> <span data-ttu-id="7bd01-1107">Si noti che il **DefaultValue**, **MaxLength**, **FixedLength**, **precisione**, **scalabilità**,  **Unicode**, e **regole di confronto** gli attributi sono applicabili solo alle **semplici EDM**.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1107">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="7bd01-1108">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="7bd01-1108">Attribute Name</span></span>                                                     | <span data-ttu-id="7bd01-1109">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="7bd01-1109">Is Required</span></span> | <span data-ttu-id="7bd01-1110">Valore</span><span class="sxs-lookup"><span data-stu-id="7bd01-1110">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="7bd01-1111">**Type**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1111">**Type**</span></span>                                                           | <span data-ttu-id="7bd01-1112">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-1112">No</span></span>          | <span data-ttu-id="7bd01-1113">Nome del tipo a cui si fa riferimento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1113">The name of the type being referenced.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="7bd01-1114">**Ammette valori null**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1114">**Nullable**</span></span>                                                       | <span data-ttu-id="7bd01-1115">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-1115">No</span></span>          | <span data-ttu-id="7bd01-1116">**True** (valore predefinito) o **False** a seconda del fatto che la proprietà può avere un valore null.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1116">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="7bd01-1117">> Nella versione 1 CSDL deve avere una proprietà del tipo complesso `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1117">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="7bd01-1118">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1118">**DefaultValue**</span></span>                                                   | <span data-ttu-id="7bd01-1119">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-1119">No</span></span>          | <span data-ttu-id="7bd01-1120">Valore predefinito della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1120">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="7bd01-1121">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1121">**MaxLength**</span></span>                                                      | <span data-ttu-id="7bd01-1122">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-1122">No</span></span>          | <span data-ttu-id="7bd01-1123">Lunghezza massima del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1123">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="7bd01-1124">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1124">**FixedLength**</span></span>                                                    | <span data-ttu-id="7bd01-1125">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-1125">No</span></span>          | <span data-ttu-id="7bd01-1126">**True** oppure **False** a seconda se il valore della proprietà essere archiviato come una stringa di lunghezza fissa.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1126">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="7bd01-1127">**Precisione**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1127">**Precision**</span></span>                                                      | <span data-ttu-id="7bd01-1128">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-1128">No</span></span>          | <span data-ttu-id="7bd01-1129">Precisione del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1129">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="7bd01-1130">**Scala**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1130">**Scale**</span></span>                                                          | <span data-ttu-id="7bd01-1131">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-1131">No</span></span>          | <span data-ttu-id="7bd01-1132">Scala del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1132">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="7bd01-1133">**SRID**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1133">**SRID**</span></span>                                                           | <span data-ttu-id="7bd01-1134">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-1134">No</span></span>          | <span data-ttu-id="7bd01-1135">Identificatore di riferimento spaziale del sistema.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1135">Spatial System Reference Identifier.</span></span> <span data-ttu-id="7bd01-1136">Valido solo per le proprietà di tipi spaziali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1136">Valid only for properties of spatial types.</span></span> <span data-ttu-id="7bd01-1137">Per altre informazioni, vedere [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="7bd01-1137">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="7bd01-1138">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1138">**Unicode**</span></span>                                                        | <span data-ttu-id="7bd01-1139">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-1139">No</span></span>          | <span data-ttu-id="7bd01-1140">**True** oppure **False** a seconda se il valore della proprietà essere archiviato come stringa Unicode.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1140">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="7bd01-1141">**regole di confronto**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1141">**Collation**</span></span>                                                      | <span data-ttu-id="7bd01-1142">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-1142">No</span></span>          | <span data-ttu-id="7bd01-1143">Stringa che specifica la sequenza di confronto da utilizzare nell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1143">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="7bd01-1144">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **CollectionType** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1144">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="7bd01-1145">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1145">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-1146">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1146">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="7bd01-1147">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-1147">Example</span></span>

<span data-ttu-id="7bd01-1148">L'esempio seguente illustra una funzione definita dal modello che utilizza il **TypeRef** elemento (come figlio di un **CollectionType** elemento) per specificare che la funzione accetta una raccolta di  **Reparto** i tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1148">The following example shows a model-defined function that uses the **TypeRef** element (as a child of a **CollectionType** element) to specify that the function accepts a collection of **Department** entity types.</span></span>

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
 

 

## <a name="using-element-csdl"></a><span data-ttu-id="7bd01-1149">Elemento Using (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-1149">Using Element (CSDL)</span></span>

<span data-ttu-id="7bd01-1150">Il **Using** elemento schema conceptual schema definition Language (CSDL) Importa il contenuto di un modello concettuale che esiste in uno spazio dei nomi diversi.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1150">The **Using** element in conceptual schema definition language (CSDL) imports the contents of a conceptual model that exists in a different namespace.</span></span> <span data-ttu-id="7bd01-1151">Impostando il valore della **Namespace** attributo, è possibile fare riferimento a tipi di entità, tipi complessi e tipi di associazione definiti in un altro modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1151">By setting the value of the **Namespace** attribute, you can refer to entity types, complex types, and association types that are defined in another conceptual model.</span></span> <span data-ttu-id="7bd01-1152">Più di un **Using** elemento può essere un figlio di un **Schema** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1152">More than one **Using** element can be a child of a **Schema** element.</span></span>

> [!NOTE]
> <span data-ttu-id="7bd01-1153">Il **Using** elemento in CSDL non funziona esattamente come una **usando** istruzione in un linguaggio di programmazione.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1153">The **Using** element in CSDL does not function exactly like a **using** statement in a programming language.</span></span> <span data-ttu-id="7bd01-1154">Tramite l'importazione di uno spazio dei nomi con un **usando** istruzione in un linguaggio di programmazione, si influisce sugli oggetti nello spazio dei nomi originale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1154">By importing a namespace with a **using** statement in a programming language, you do not affect objects in the original namespace.</span></span> <span data-ttu-id="7bd01-1155">In CSDL, uno spazio dei nomi importato può contenere un tipo di entità derivato da un tipo di entità nello spazio dei nomi originale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1155">In CSDL, an imported namespace can contain an entity type that is derived from an entity type in the original namespace.</span></span> <span data-ttu-id="7bd01-1156">Ciò può influire sui set di entità dichiarati nello spazio dei nomi originale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1156">This can affect entity sets declared in the original namespace.</span></span>

 

<span data-ttu-id="7bd01-1157">Il **Using** elemento può avere elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="7bd01-1157">The **Using** element can have the following child elements:</span></span>

-   <span data-ttu-id="7bd01-1158">Documentazione (zero o un elemento consentito)</span><span class="sxs-lookup"><span data-stu-id="7bd01-1158">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="7bd01-1159">Elementi Annotation (zero o più elementi consentiti)</span><span class="sxs-lookup"><span data-stu-id="7bd01-1159">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="7bd01-1160">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-1160">Applicable Attributes</span></span>

<span data-ttu-id="7bd01-1161">Nella tabella seguente vengono descritti gli attributi possono essere applicati per il **Using** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1161">The table below describes the attributes can be applied to the **Using** element.</span></span>

| <span data-ttu-id="7bd01-1162">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="7bd01-1162">Attribute Name</span></span> | <span data-ttu-id="7bd01-1163">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="7bd01-1163">Is Required</span></span> | <span data-ttu-id="7bd01-1164">Valore</span><span class="sxs-lookup"><span data-stu-id="7bd01-1164">Value</span></span>                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="7bd01-1165">**Spazio dei nomi**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1165">**Namespace**</span></span>  | <span data-ttu-id="7bd01-1166">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-1166">Yes</span></span>         | <span data-ttu-id="7bd01-1167">Nome dello spazio dei nomi importato.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1167">The name of the imported namespace.</span></span>                                                                                                                                                |
| <span data-ttu-id="7bd01-1168">**Alias**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1168">**Alias**</span></span>      | <span data-ttu-id="7bd01-1169">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-1169">Yes</span></span>         | <span data-ttu-id="7bd01-1170">Identificatore utilizzato al posto del nome dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1170">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="7bd01-1171">Anche se questo attributo è obbligatorio, non è necessario utilizzarlo al posto del nome dello spazio dei nomi per qualificare nomi di oggetti.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1171">Although this attribute is required, it is not required that it be used in place of the namespace name to qualify object names.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="7bd01-1172">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **Using** elemento.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1172">Any number of annotation attributes (custom XML attributes) may be applied to the **Using** element.</span></span> <span data-ttu-id="7bd01-1173">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1173">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="7bd01-1174">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1174">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="7bd01-1175">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-1175">Example</span></span>

<span data-ttu-id="7bd01-1176">Nell'esempio seguente viene illustrato il **Using** elemento utilizzato per importare uno spazio dei nomi definito altrove.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1176">The following example demonstrates the **Using** element being used to import a namespace that is defined elsewhere.</span></span> <span data-ttu-id="7bd01-1177">Si noti che lo spazio dei nomi per il **Schema** elemento mostrato è `BooksModel`.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1177">Note that the namespace for the **Schema** element shown is `BooksModel`.</span></span> <span data-ttu-id="7bd01-1178">Il `Address` proprietà il `Publisher` **EntityType** è un tipo complesso definito nel `ExtendedBooksModel` dello spazio dei nomi (importati con il **Using** elemento).</span><span class="sxs-lookup"><span data-stu-id="7bd01-1178">The `Address` property on the `Publisher`**EntityType** is a complex type that is defined in the `ExtendedBooksModel` namespace (imported with the **Using** element).</span></span>

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
 

 

## <a name="annotation-attributes-csdl"></a><span data-ttu-id="7bd01-1179">Attributi di annotazione (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-1179">Annotation Attributes (CSDL)</span></span>

<span data-ttu-id="7bd01-1180">Gli attributi di annotazione in Conceptual Schema Definition Language (CSDL) sono attributi XML personalizzati nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1180">Annotation attributes in conceptual schema definition language (CSDL) are custom XML attributes in the conceptual model.</span></span> <span data-ttu-id="7bd01-1181">Oltre ad avere una struttura XML valida, per gli attributi di annotazione hanno le caratteristiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="7bd01-1181">In addition to having valid XML structure, the following must be true of annotation attributes:</span></span>

-   <span data-ttu-id="7bd01-1182">Gli attributi di annotazione non devono trovarsi in spazi dei nomi XML riservati a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1182">Annotation attributes must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="7bd01-1183">È possibile applicare più attributi di annotazione a un determinato elemento CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1183">More than one annotation attribute may be applied to a given CSDL element.</span></span>
-   <span data-ttu-id="7bd01-1184">I nomi completi di due attributi di annotazione non devono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1184">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="7bd01-1185">Gli attributi di annotazione possono essere utilizzati per fornire metadati aggiuntivi sugli elementi in un modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1185">Annotation attributes can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="7bd01-1186">Metadati contenuti negli elementi di annotazione sono accessibili in fase di esecuzione usando le classi nello spazio dei nomi System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1186">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="7bd01-1187">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-1187">Example</span></span>

<span data-ttu-id="7bd01-1188">L'esempio seguente mostra un' **EntityType** elemento con un attributo di annotazione (**CustomAttribute**).</span><span class="sxs-lookup"><span data-stu-id="7bd01-1188">The following example shows an **EntityType** element with an annotation attribute (**CustomAttribute**).</span></span> <span data-ttu-id="7bd01-1189">Nell'esempio viene inoltre mostrato un elemento di annotazione applicato all'elemento del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1189">The example also shows an annotation element applied to the entity type element.</span></span>

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
 

<span data-ttu-id="7bd01-1190">Il codice seguente recupera i metadati dell'attributo di annotazione e li scrive nella console:</span><span class="sxs-lookup"><span data-stu-id="7bd01-1190">The following code retrieves the metadata in the annotation attribute and writes it to the console:</span></span>

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
 

<span data-ttu-id="7bd01-1191">Nel codice riportato in precedenza si presuppone che il file `School.csdl` si trovi nella directory di output del progetto e che al progetto siano state aggiunte le istruzioni `Imports` e `Using` seguenti:</span><span class="sxs-lookup"><span data-stu-id="7bd01-1191">The code above assumes that the `School.csdl` file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a><span data-ttu-id="7bd01-1192">Elementi di annotazione (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-1192">Annotation Elements (CSDL)</span></span>

<span data-ttu-id="7bd01-1193">Gli elementi Annotation in Conceptual Schema Definition Language (CSDL) sono elementi XML personalizzati nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1193">Annotation elements in conceptual schema definition language (CSDL) are custom XML elements in the conceptual model.</span></span> <span data-ttu-id="7bd01-1194">Oltre ad avere una struttura XML valida, gli elementi Annotation hanno le caratteristiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="7bd01-1194">In addition to having valid XML structure, the following must be true of annotation elements:</span></span>

-   <span data-ttu-id="7bd01-1195">Gli elementi Annotation non devono trovarsi in spazi dei nomi XML riservati a CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1195">Annotation elements must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="7bd01-1196">Più elementi Annotation possono essere figli di un dato elemento CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1196">More than one annotation element may be a child of a given CSDL element.</span></span>
-   <span data-ttu-id="7bd01-1197">I nomi completi di due elementi Annotation non devono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1197">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="7bd01-1198">Gli elementi Annotation devono apparire dopo tutti gli altri elementi figlio di un dato elemento CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1198">Annotation elements must appear after all other child elements of a given CSDL element.</span></span>

<span data-ttu-id="7bd01-1199">Gli elementi Annotation possono essere utilizzati per fornire metadati aggiuntivi sugli elementi in un modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1199">Annotation elements can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="7bd01-1200">A partire da .NET Framework versione 4, i metadati contenuti in elementi annotation sono accessibile in fase di esecuzione usando le classi nello spazio dei nomi System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1200">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="7bd01-1201">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-1201">Example</span></span>

<span data-ttu-id="7bd01-1202">L'esempio seguente mostra un' **EntityType** elemento con un elemento annotation (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="7bd01-1202">The following example shows an **EntityType** element with an annotation element (**CustomElement**).</span></span> <span data-ttu-id="7bd01-1203">Nell'esempio viene inoltre mostrato un attributo di annotazione applicato all'elemento del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1203">The example also show an annotation attribute applied to the entity type element.</span></span>

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
 

<span data-ttu-id="7bd01-1204">Il codice seguente recupera i metadati dell'elemento Annotation e li scrive nella console:</span><span class="sxs-lookup"><span data-stu-id="7bd01-1204">The following code retrieves the metadata in the annotation element and writes it to the console:</span></span>

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
 

<span data-ttu-id="7bd01-1205">Nel codice riportato in precedenza si presuppone che il file School.csdl si trovi nella directory di output del progetto e che al progetto siano state aggiunte le istruzioni `Imports` e `Using` seguenti:</span><span class="sxs-lookup"><span data-stu-id="7bd01-1205">The code above assumes that the School.csdl file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a><span data-ttu-id="7bd01-1206">Tipi del modello concettuale (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-1206">Conceptual Model Types (CSDL)</span></span>

<span data-ttu-id="7bd01-1207">Linguaggio di definizione schema concettuale (CSDL) supporta un set di tipi di dati primitivi astratti, chiamato **semplici EDM**, che definiscono le proprietà in un modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1207">Conceptual schema definition language (CSDL) supports a set of abstract primitive data types, called **EDMSimpleTypes**, that define properties in a conceptual model.</span></span> <span data-ttu-id="7bd01-1208">**Semplici EDM** sono proxy per i tipi di dati primitivi supportati in ambiente di hosting o nell'archivio.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1208">**EDMSimpleTypes** are proxies for primitive data types that are supported in the storage or hosting environment.</span></span>

<span data-ttu-id="7bd01-1209">Nella tabella seguente vengono elencati i tipi di dati primitivi supportati da CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1209">The table below lists the primitive data types that are supported by CSDL.</span></span> <span data-ttu-id="7bd01-1210">Vengono inoltre elencati i facet che possono essere applicati a ciascun **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1210">The table also lists the facets that can be applied to each **EDMSimpleType**.</span></span>

| <span data-ttu-id="7bd01-1211">EDMSimpleType</span><span class="sxs-lookup"><span data-stu-id="7bd01-1211">EDMSimpleType</span></span>                    | <span data-ttu-id="7bd01-1212">Descrizione</span><span class="sxs-lookup"><span data-stu-id="7bd01-1212">Description</span></span>                                                | <span data-ttu-id="7bd01-1213">Facet applicabili</span><span class="sxs-lookup"><span data-stu-id="7bd01-1213">Applicable Facets</span></span>                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| <span data-ttu-id="7bd01-1214">**EDM. Binary**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1214">**Edm.Binary**</span></span>                   | <span data-ttu-id="7bd01-1215">Contiene dati binari.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1215">Contains binary data.</span></span>                                      | <span data-ttu-id="7bd01-1216">MaxLength, FixedLength, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="7bd01-1216">MaxLength, FixedLength, Nullable, Default</span></span>                                |
| <span data-ttu-id="7bd01-1217">**Boolean**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1217">**Edm.Boolean**</span></span>                  | <span data-ttu-id="7bd01-1218">Contiene il valore **true** oppure **false**.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1218">Contains the value **true** or **false**.</span></span>                  | <span data-ttu-id="7bd01-1219">Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="7bd01-1219">Nullable, Default</span></span>                                                        |
| <span data-ttu-id="7bd01-1220">**Edm.Byte**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1220">**Edm.Byte**</span></span>                     | <span data-ttu-id="7bd01-1221">Contiene un Unsigned Integer a 8 bit.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1221">Contains an unsigned 8-bit integer value.</span></span>                  | <span data-ttu-id="7bd01-1222">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="7bd01-1222">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="7bd01-1223">**EDM**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1223">**Edm.DateTime**</span></span>                 | <span data-ttu-id="7bd01-1224">Rappresenta una data e un'ora.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1224">Represents a date and time.</span></span>                                | <span data-ttu-id="7bd01-1225">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="7bd01-1225">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="7bd01-1226">**DateTimeOffset**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1226">**Edm.DateTimeOffset**</span></span>           | <span data-ttu-id="7bd01-1227">Contiene una data e un'ora come offset in minuti rispetto all'ora GMT.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1227">Contains a date and time as an offset in minutes from GMT.</span></span> | <span data-ttu-id="7bd01-1228">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="7bd01-1228">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="7bd01-1229">**Decimal**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1229">**Edm.Decimal**</span></span>                  | <span data-ttu-id="7bd01-1230">Contiene un valore numerico con scala e precisione fisse.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1230">Contains a numeric value with fixed precision and scale.</span></span>   | <span data-ttu-id="7bd01-1231">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="7bd01-1231">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="7bd01-1232">**EDM. Double**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1232">**Edm.Double**</span></span>                   | <span data-ttu-id="7bd01-1233">Contiene un numero con precisione a 15 cifre a virgola mobile</span><span class="sxs-lookup"><span data-stu-id="7bd01-1233">Contains a floating point number with 15-digit precision</span></span>   | <span data-ttu-id="7bd01-1234">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="7bd01-1234">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="7bd01-1235">**Edm.Float**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1235">**Edm.Float**</span></span>                    | <span data-ttu-id="7bd01-1236">Contiene un numero a virgola mobile con precisione a 7 cifre.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1236">Contains a floating point number with 7-digit precision.</span></span>   | <span data-ttu-id="7bd01-1237">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="7bd01-1237">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="7bd01-1238">**EDM. GUID**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1238">**Edm.Guid**</span></span>                     | <span data-ttu-id="7bd01-1239">Contiene un identificatore univoco a 16 byte.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1239">Contains a 16-byte unique identifier.</span></span>                      | <span data-ttu-id="7bd01-1240">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="7bd01-1240">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="7bd01-1241">**Edm.Int16**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1241">**Edm.Int16**</span></span>                    | <span data-ttu-id="7bd01-1242">Contiene un Signed Integer a 16 bit.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1242">Contains a signed 16-bit integer value.</span></span>                    | <span data-ttu-id="7bd01-1243">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="7bd01-1243">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="7bd01-1244">**Edm.Int32**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1244">**Edm.Int32**</span></span>                    | <span data-ttu-id="7bd01-1245">Contiene un Signed Integer a 32 bit.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1245">Contains a signed 32-bit integer value.</span></span>                    | <span data-ttu-id="7bd01-1246">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="7bd01-1246">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="7bd01-1247">**Edm.Int64**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1247">**Edm.Int64**</span></span>                    | <span data-ttu-id="7bd01-1248">Contiene un Signed Integer a 64 bit.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1248">Contains a signed 64-bit integer value.</span></span>                    | <span data-ttu-id="7bd01-1249">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="7bd01-1249">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="7bd01-1250">**Edm.SByte**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1250">**Edm.SByte**</span></span>                    | <span data-ttu-id="7bd01-1251">Contiene un Signed Integer a 8 bit.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1251">Contains a signed 8-bit integer value.</span></span>                     | <span data-ttu-id="7bd01-1252">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="7bd01-1252">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="7bd01-1253">**EDM. String**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1253">**Edm.String**</span></span>                   | <span data-ttu-id="7bd01-1254">Contiene dati di tipo carattere.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1254">Contains character data.</span></span>                                   | <span data-ttu-id="7bd01-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="7bd01-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span></span> |
| <span data-ttu-id="7bd01-1256">**EDM**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1256">**Edm.Time**</span></span>                     | <span data-ttu-id="7bd01-1257">Contiene un'ora del giorno.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1257">Contains a time of day.</span></span>                                    | <span data-ttu-id="7bd01-1258">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="7bd01-1258">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="7bd01-1259">**Geography**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1259">**Edm.Geography**</span></span>                |                                                            | <span data-ttu-id="7bd01-1260">Ammette valori null, impostazione predefinita, l'identificatore SRID</span><span class="sxs-lookup"><span data-stu-id="7bd01-1260">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="7bd01-1261">**EDM. geographypoint**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1261">**Edm.GeographyPoint**</span></span>           |                                                            | <span data-ttu-id="7bd01-1262">Ammette valori null, impostazione predefinita, l'identificatore SRID</span><span class="sxs-lookup"><span data-stu-id="7bd01-1262">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="7bd01-1263">**Edm.GeographyLineString**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1263">**Edm.GeographyLineString**</span></span>      |                                                            | <span data-ttu-id="7bd01-1264">Ammette valori null, impostazione predefinita, l'identificatore SRID</span><span class="sxs-lookup"><span data-stu-id="7bd01-1264">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="7bd01-1265">**Geographypolygon**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1265">**Edm.GeographyPolygon**</span></span>         |                                                            | <span data-ttu-id="7bd01-1266">Ammette valori null, impostazione predefinita, l'identificatore SRID</span><span class="sxs-lookup"><span data-stu-id="7bd01-1266">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="7bd01-1267">**Edm.GeographyMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1267">**Edm.GeographyMultiPoint**</span></span>      |                                                            | <span data-ttu-id="7bd01-1268">Ammette valori null, impostazione predefinita, l'identificatore SRID</span><span class="sxs-lookup"><span data-stu-id="7bd01-1268">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="7bd01-1269">**Edm.GeographyMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1269">**Edm.GeographyMultiLineString**</span></span> |                                                            | <span data-ttu-id="7bd01-1270">Ammette valori null, impostazione predefinita, l'identificatore SRID</span><span class="sxs-lookup"><span data-stu-id="7bd01-1270">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="7bd01-1271">**Edm.GeographyMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1271">**Edm.GeographyMultiPolygon**</span></span>    |                                                            | <span data-ttu-id="7bd01-1272">Ammette valori null, impostazione predefinita, l'identificatore SRID</span><span class="sxs-lookup"><span data-stu-id="7bd01-1272">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="7bd01-1273">**Edm.GeographyCollection**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1273">**Edm.GeographyCollection**</span></span>      |                                                            | <span data-ttu-id="7bd01-1274">Ammette valori null, impostazione predefinita, l'identificatore SRID</span><span class="sxs-lookup"><span data-stu-id="7bd01-1274">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="7bd01-1275">**EDM**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1275">**Edm.Geometry**</span></span>                 |                                                            | <span data-ttu-id="7bd01-1276">Ammette valori null, impostazione predefinita, l'identificatore SRID</span><span class="sxs-lookup"><span data-stu-id="7bd01-1276">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="7bd01-1277">**Edm.GeometryPoint**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1277">**Edm.GeometryPoint**</span></span>            |                                                            | <span data-ttu-id="7bd01-1278">Ammette valori null, impostazione predefinita, l'identificatore SRID</span><span class="sxs-lookup"><span data-stu-id="7bd01-1278">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="7bd01-1279">**Edm.GeometryLineString**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1279">**Edm.GeometryLineString**</span></span>       |                                                            | <span data-ttu-id="7bd01-1280">Ammette valori null, impostazione predefinita, l'identificatore SRID</span><span class="sxs-lookup"><span data-stu-id="7bd01-1280">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="7bd01-1281">**Edm.GeometryPolygon**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1281">**Edm.GeometryPolygon**</span></span>          |                                                            | <span data-ttu-id="7bd01-1282">Ammette valori null, impostazione predefinita, l'identificatore SRID</span><span class="sxs-lookup"><span data-stu-id="7bd01-1282">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="7bd01-1283">**Edm.GeometryMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1283">**Edm.GeometryMultiPoint**</span></span>       |                                                            | <span data-ttu-id="7bd01-1284">Ammette valori null, impostazione predefinita, l'identificatore SRID</span><span class="sxs-lookup"><span data-stu-id="7bd01-1284">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="7bd01-1285">**Edm.GeometryMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1285">**Edm.GeometryMultiLineString**</span></span>  |                                                            | <span data-ttu-id="7bd01-1286">Ammette valori null, impostazione predefinita, l'identificatore SRID</span><span class="sxs-lookup"><span data-stu-id="7bd01-1286">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="7bd01-1287">**Edm.GeometryMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1287">**Edm.GeometryMultiPolygon**</span></span>     |                                                            | <span data-ttu-id="7bd01-1288">Ammette valori null, impostazione predefinita, l'identificatore SRID</span><span class="sxs-lookup"><span data-stu-id="7bd01-1288">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="7bd01-1289">**Edm.GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1289">**Edm.GeometryCollection**</span></span>       |                                                            | <span data-ttu-id="7bd01-1290">Ammette valori null, impostazione predefinita, l'identificatore SRID</span><span class="sxs-lookup"><span data-stu-id="7bd01-1290">Nullable, Default, SRID</span></span>                                                  |

## <a name="facets-csdl"></a><span data-ttu-id="7bd01-1291">Facet (CSDL)</span><span class="sxs-lookup"><span data-stu-id="7bd01-1291">Facets (CSDL)</span></span>

<span data-ttu-id="7bd01-1292">I facet in Conceptual Schema Definition Language (CSDL) rappresentano vincoli sulle proprietà di tipi di entità e di tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1292">Facets in conceptual schema definition language (CSDL) represent constraints on properties of entity types and complex types.</span></span> <span data-ttu-id="7bd01-1293">I facet appaiono come attributi XML negli elementi CSDL seguenti:</span><span class="sxs-lookup"><span data-stu-id="7bd01-1293">Facets appear as XML attributes on the following CSDL elements:</span></span>

-   <span data-ttu-id="7bd01-1294">Proprietà</span><span class="sxs-lookup"><span data-stu-id="7bd01-1294">Property</span></span>
-   <span data-ttu-id="7bd01-1295">TypeRef</span><span class="sxs-lookup"><span data-stu-id="7bd01-1295">TypeRef</span></span>
-   <span data-ttu-id="7bd01-1296">Parametro</span><span class="sxs-lookup"><span data-stu-id="7bd01-1296">Parameter</span></span>

<span data-ttu-id="7bd01-1297">Nella tabella seguente vengono descritti i facet supportati in CSDL.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1297">The following table describes the facets that are supported in CSDL.</span></span> <span data-ttu-id="7bd01-1298">Tutti i facet sono facoltativi.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1298">All facets are optional.</span></span> <span data-ttu-id="7bd01-1299">Alcuni facet elencati di seguito vengono utilizzate da Entity Framework durante la generazione di un database da un modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1299">Some facets listed below are used by the Entity Framework when generating a database from a conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="7bd01-1300">Per informazioni sui tipi di dati in un modello concettuale, vedere i tipi di modello concettuale (CSDL).</span><span class="sxs-lookup"><span data-stu-id="7bd01-1300">For information about data types in a conceptual model, see Conceptual Model Types (CSDL).</span></span>

| <span data-ttu-id="7bd01-1301">Facet</span><span class="sxs-lookup"><span data-stu-id="7bd01-1301">Facet</span></span>               | <span data-ttu-id="7bd01-1302">Descrizione</span><span class="sxs-lookup"><span data-stu-id="7bd01-1302">Description</span></span>                                                                                                                                                                                                                                                   | <span data-ttu-id="7bd01-1303">Si applica a</span><span class="sxs-lookup"><span data-stu-id="7bd01-1303">Applies to</span></span>                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="7bd01-1304">Utilizzato per la generazione di database.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1304">Used for the database generation</span></span> | <span data-ttu-id="7bd01-1305">Utilizzato dal runtime</span><span class="sxs-lookup"><span data-stu-id="7bd01-1305">Used by the runtime</span></span> |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| <span data-ttu-id="7bd01-1306">**regole di confronto**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1306">**Collation**</span></span>       | <span data-ttu-id="7bd01-1307">Specifica la sequenza di ordinamento da usare quando si eseguono operazioni di confronto e di ordinamento su valori della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1307">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                               | <span data-ttu-id="7bd01-1308">**EDM. String**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1308">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="7bd01-1309">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-1309">Yes</span></span>                              | <span data-ttu-id="7bd01-1310">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-1310">No</span></span>                  |
| <span data-ttu-id="7bd01-1311">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1311">**ConcurrencyMode**</span></span> | <span data-ttu-id="7bd01-1312">Indica che il valore della proprietà deve essere usato per le verifiche della concorrenza ottimistica.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1312">Indicates that the value of the property should be used for optimistic concurrency checks.</span></span>                                                                                                                                                                    | <span data-ttu-id="7bd01-1313">Tutti i **EDMSimpleType** proprietà</span><span class="sxs-lookup"><span data-stu-id="7bd01-1313">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="7bd01-1314">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-1314">No</span></span>                               | <span data-ttu-id="7bd01-1315">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-1315">Yes</span></span>                 |
| <span data-ttu-id="7bd01-1316">**Default**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1316">**Default**</span></span>         | <span data-ttu-id="7bd01-1317">Specifica il valore predefinito della proprietà se durante la creazione di istanze non viene fornito alcun valore.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1317">Specifies the default value of the property if no value is supplied upon instantiation.</span></span>                                                                                                                                                                       | <span data-ttu-id="7bd01-1318">Tutti i **EDMSimpleType** proprietà</span><span class="sxs-lookup"><span data-stu-id="7bd01-1318">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="7bd01-1319">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-1319">Yes</span></span>                              | <span data-ttu-id="7bd01-1320">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-1320">Yes</span></span>                 |
| <span data-ttu-id="7bd01-1321">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1321">**FixedLength**</span></span>     | <span data-ttu-id="7bd01-1322">Specifica se la lunghezza del valore della proprietà può variare.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1322">Specifies whether the length of the property value can vary.</span></span>                                                                                                                                                                                                  | <span data-ttu-id="7bd01-1323">**EDM. Binary**, **Edm. String**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1323">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="7bd01-1324">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-1324">Yes</span></span>                              | <span data-ttu-id="7bd01-1325">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-1325">No</span></span>                  |
| <span data-ttu-id="7bd01-1326">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1326">**MaxLength**</span></span>       | <span data-ttu-id="7bd01-1327">Specifica la lunghezza massima del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1327">Specifies the maximum length of the property value.</span></span>                                                                                                                                                                                                           | <span data-ttu-id="7bd01-1328">**EDM. Binary**, **Edm. String**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1328">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="7bd01-1329">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-1329">Yes</span></span>                              | <span data-ttu-id="7bd01-1330">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-1330">No</span></span>                  |
| <span data-ttu-id="7bd01-1331">**Ammette valori null**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1331">**Nullable**</span></span>        | <span data-ttu-id="7bd01-1332">Specifica se la proprietà può avere una **null** valore.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1332">Specifies whether the property can have a **null** value.</span></span>                                                                                                                                                                                                     | <span data-ttu-id="7bd01-1333">Tutti i **EDMSimpleType** proprietà</span><span class="sxs-lookup"><span data-stu-id="7bd01-1333">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="7bd01-1334">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-1334">Yes</span></span>                              | <span data-ttu-id="7bd01-1335">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-1335">Yes</span></span>                 |
| <span data-ttu-id="7bd01-1336">**Precisione**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1336">**Precision**</span></span>       | <span data-ttu-id="7bd01-1337">Per le proprietà di tipo **decimale**, specifica il numero di cifre può avere un valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1337">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="7bd01-1338">Per le proprietà di tipo **tempo**, **DateTime**, e **DateTimeOffset**, specifica il numero di cifre per la parte frazionaria di secondi del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1338">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the property value.</span></span> | <span data-ttu-id="7bd01-1339">**EDM**, **DateTimeOffset**, **decimal**, **EDM**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span></span>                                                                                                                                                                                                                                                                                                              | <span data-ttu-id="7bd01-1340">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-1340">Yes</span></span>                              | <span data-ttu-id="7bd01-1341">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-1341">No</span></span>                  |
| <span data-ttu-id="7bd01-1342">**Scala**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1342">**Scale**</span></span>           | <span data-ttu-id="7bd01-1343">Specifica il numero di cifre a destra del separatore decimale per il valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1343">Specifies the number of digits to the right of the decimal point for the property value.</span></span>                                                                                                                                                                      | <span data-ttu-id="7bd01-1344">**Decimal**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1344">**Edm.Decimal**</span></span>                                                                                                                                                                                                                                                                                                                                                                      | <span data-ttu-id="7bd01-1345">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-1345">Yes</span></span>                              | <span data-ttu-id="7bd01-1346">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-1346">No</span></span>                  |
| <span data-ttu-id="7bd01-1347">**SRID**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1347">**SRID**</span></span>            | <span data-ttu-id="7bd01-1348">Specifica l'ID sistema spaziale riferimento del sistema.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1348">Specifies the Spatial System Reference System ID.</span></span> <span data-ttu-id="7bd01-1349">Per altre informazioni, vedere [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="7bd01-1349">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span>                                                              | <span data-ttu-id="7bd01-1350">**Geography, EDM. geographypoint, Edm.GeographyLineString, geographypolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, EDM, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1350">**Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span></span> | <span data-ttu-id="7bd01-1351">No</span><span class="sxs-lookup"><span data-stu-id="7bd01-1351">No</span></span>                               | <span data-ttu-id="7bd01-1352">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-1352">Yes</span></span>                 |
| <span data-ttu-id="7bd01-1353">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1353">**Unicode**</span></span>         | <span data-ttu-id="7bd01-1354">Viene indicato se il valore della proprietà viene archiviato come Unicode.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1354">Indicates whether the property value is stored as Unicode.</span></span>                                                                                                                                                                                                    | <span data-ttu-id="7bd01-1355">**EDM. String**</span><span class="sxs-lookup"><span data-stu-id="7bd01-1355">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="7bd01-1356">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-1356">Yes</span></span>                              | <span data-ttu-id="7bd01-1357">Yes</span><span class="sxs-lookup"><span data-stu-id="7bd01-1357">Yes</span></span>                 |

>[!NOTE]
> <span data-ttu-id="7bd01-1358">Durante la generazione di un database da un modello concettuale, la procedura guidata Crea Database riconosceranno il valore della **StoreGeneratedPattern** dell'attributo su un **proprietà** elemento se è nel seguente spazio dei nomi: http://schemas.microsoft.com/ado/2009/02/edm/annotation .</span><span class="sxs-lookup"><span data-stu-id="7bd01-1358">When generating a database from a conceptual model, the Generate Database Wizard will recognize the value of the **StoreGeneratedPattern** attribute on a **Property** element if it is in the following namespace: http://schemas.microsoft.com/ado/2009/02/edm/annotation.</span></span> <span data-ttu-id="7bd01-1359">I valori supportati per l'attributo vengono **Identity** e **calcolata**.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1359">The supported values for the attribute are **Identity** and **Computed**.</span></span> <span data-ttu-id="7bd01-1360">Un valore pari **identità** produrrà una colonna di database con un valore identity generato nel database.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1360">A value of **Identity** will produce a database column with an identity value that is generated in the database.</span></span> <span data-ttu-id="7bd01-1361">Un valore pari **calcolata** produrrà una colonna con un valore calcolato nel database.</span><span class="sxs-lookup"><span data-stu-id="7bd01-1361">A value of **Computed** will produce a column with a value that is computed in the database.</span></span>

### <a name="example"></a><span data-ttu-id="7bd01-1362">Esempio</span><span class="sxs-lookup"><span data-stu-id="7bd01-1362">Example</span></span>

<span data-ttu-id="7bd01-1363">Nell'esempio seguente vengono mostrati i facet applicati alle proprietà di un tipo di entità:</span><span class="sxs-lookup"><span data-stu-id="7bd01-1363">The following example shows facets applied to the properties of an entity type:</span></span>

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
