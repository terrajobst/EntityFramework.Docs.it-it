---
title: Glossario Entity Framework-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
uid: ef6/resources/glossary
ms.openlocfilehash: df0da4a68b3d2c882d9673417ee5fe335eccae2b
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/15/2020
ms.locfileid: "79402201"
---
# <a name="entity-framework-glossary"></a><span data-ttu-id="8aa34-102">Glossario Entity Framework</span><span class="sxs-lookup"><span data-stu-id="8aa34-102">Entity Framework Glossary</span></span>
## <a name="code-first"></a><span data-ttu-id="8aa34-103">Code First</span><span class="sxs-lookup"><span data-stu-id="8aa34-103">Code First</span></span>
<span data-ttu-id="8aa34-104">Creazione di un modello di Entity Framework utilizzando il codice.</span><span class="sxs-lookup"><span data-stu-id="8aa34-104">Creating an Entity Framework model using code.</span></span> <span data-ttu-id="8aa34-105">Il modello può essere destinato a un database esistente o a un nuovo database.</span><span class="sxs-lookup"><span data-stu-id="8aa34-105">The model can target an existing database or a new database.</span></span>

## <a name="context"></a><span data-ttu-id="8aa34-106">Context</span><span class="sxs-lookup"><span data-stu-id="8aa34-106">Context</span></span>
<span data-ttu-id="8aa34-107">Classe che rappresenta una sessione con il database, che consente di eseguire query e salvare i dati.</span><span class="sxs-lookup"><span data-stu-id="8aa34-107">A class that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="8aa34-108">Un contesto deriva dalla classe DbContext o ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="8aa34-108">A context derives from the DbContext or ObjectContext class.</span></span>

## <a name="convention-code-first"></a><span data-ttu-id="8aa34-109">Convenzione (Code First)</span><span class="sxs-lookup"><span data-stu-id="8aa34-109">Convention (Code First)</span></span>
<span data-ttu-id="8aa34-110">Regola che Entity Framework utilizza per dedurre la forma del modello dalle classi.</span><span class="sxs-lookup"><span data-stu-id="8aa34-110">A rule that Entity Framework uses to infer the shape of you model from your classes.</span></span>

## <a name="database-first"></a><span data-ttu-id="8aa34-111">Database First</span><span class="sxs-lookup"><span data-stu-id="8aa34-111">Database First</span></span>
<span data-ttu-id="8aa34-112">Creazione di un modello di Entity Framework, usando la finestra di progettazione EF, destinata a un database esistente.</span><span class="sxs-lookup"><span data-stu-id="8aa34-112">Creating an Entity Framework model, using the EF Designer, that targets an existing database.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="8aa34-113">Caricamento eager</span><span class="sxs-lookup"><span data-stu-id="8aa34-113">Eager loading</span></span>
<span data-ttu-id="8aa34-114">Modello di caricamento di dati correlati in cui una query per un tipo di entità carica anche le entità correlate come parte della query.</span><span class="sxs-lookup"><span data-stu-id="8aa34-114">A pattern of loading related data where a query for one type of entity also loads related entities as part of the query.</span></span>

## <a name="ef-designer"></a><span data-ttu-id="8aa34-115">EF Designer</span><span class="sxs-lookup"><span data-stu-id="8aa34-115">EF Designer</span></span>
<span data-ttu-id="8aa34-116">Finestra di progettazione visiva in Visual Studio che consente di creare un modello di Entity Framework usando caselle e linee.</span><span class="sxs-lookup"><span data-stu-id="8aa34-116">A visual designer in Visual Studio that allows you to create an Entity Framework model using boxes and lines.</span></span>

## <a name="entity"></a><span data-ttu-id="8aa34-117">Entità</span><span class="sxs-lookup"><span data-stu-id="8aa34-117">Entity</span></span>
<span data-ttu-id="8aa34-118">Classe o oggetto che rappresenta i dati dell'applicazione come clienti, prodotti e ordini.</span><span class="sxs-lookup"><span data-stu-id="8aa34-118">A class or object that represents application data such as customers, products, and orders.</span></span>

## <a name="entity-data-model"></a><span data-ttu-id="8aa34-119">Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="8aa34-119">Entity Data Model</span></span>
<span data-ttu-id="8aa34-120">Modello che descrive le entità e le relazioni tra di esse.</span><span class="sxs-lookup"><span data-stu-id="8aa34-120">A model that describes entities and the relationships between them.</span></span> <span data-ttu-id="8aa34-121">EF utilizza EDM per descrivere il modello concettuale su cui si basano i programmi per sviluppatori.</span><span class="sxs-lookup"><span data-stu-id="8aa34-121">EF uses EDM to describe the conceptual model against which the developer programs.</span></span> <span data-ttu-id="8aa34-122">EDM si basa sul modello Entity Relationship introdotto da Dr. Peter Chen.</span><span class="sxs-lookup"><span data-stu-id="8aa34-122">EDM builds on the Entity Relationship model introduced by Dr. Peter Chen.</span></span> <span data-ttu-id="8aa34-123">EDM è stato originariamente sviluppato con l'obiettivo principale di diventare il modello di dati comune in una suite di tecnologie per sviluppatori e server Microsoft.</span><span class="sxs-lookup"><span data-stu-id="8aa34-123">The EDM was originally developed with the primary goal of becoming the common data model across a suite of developer and server technologies from Microsoft.</span></span> <span data-ttu-id="8aa34-124">EDM viene utilizzato anche come parte del protocollo OData.</span><span class="sxs-lookup"><span data-stu-id="8aa34-124">EDM is also used as part of the OData protocol.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="8aa34-125">Caricamento esplicito</span><span class="sxs-lookup"><span data-stu-id="8aa34-125">Explicit loading</span></span>
<span data-ttu-id="8aa34-126">Modello di caricamento di dati correlati in cui gli oggetti correlati vengono caricati chiamando un'API.</span><span class="sxs-lookup"><span data-stu-id="8aa34-126">A pattern of loading related data where related objects are loaded by calling an API.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="8aa34-127">API Fluent</span><span class="sxs-lookup"><span data-stu-id="8aa34-127">Fluent API</span></span>
<span data-ttu-id="8aa34-128">API che può essere utilizzata per configurare un modello di Code First.</span><span class="sxs-lookup"><span data-stu-id="8aa34-128">An API that can be used to configure a Code First model.</span></span>

## <a name="foreign-key-association"></a><span data-ttu-id="8aa34-129">Associazione di chiavi esterne</span><span class="sxs-lookup"><span data-stu-id="8aa34-129">Foreign key association</span></span>
<span data-ttu-id="8aa34-130">Associazione tra entità in cui una proprietà che rappresenta la chiave esterna è inclusa nella classe dell'entità dipendente.</span><span class="sxs-lookup"><span data-stu-id="8aa34-130">An association between entities where a property that represents the foreign key is included in the class of the dependent entity.</span></span> <span data-ttu-id="8aa34-131">Il prodotto, ad esempio, contiene una proprietà CategoryId.</span><span class="sxs-lookup"><span data-stu-id="8aa34-131">For example, Product contains a CategoryId property.</span></span>

## <a name="identifying-relationship"></a><span data-ttu-id="8aa34-132">Relazione di identificazione</span><span class="sxs-lookup"><span data-stu-id="8aa34-132">Identifying relationship</span></span>
<span data-ttu-id="8aa34-133">Relazione in cui la chiave primaria dell'entità principale fa parte della chiave primaria dell'entità dipendente.</span><span class="sxs-lookup"><span data-stu-id="8aa34-133">A relationship where the primary key of the principal entity is part of the primary key of the dependent entity.</span></span> <span data-ttu-id="8aa34-134">In questo tipo di relazione l'entità dipendente non può esistere senza l'entità principale.</span><span class="sxs-lookup"><span data-stu-id="8aa34-134">In this kind of relationship, the dependent entity cannot exist without the principal entity.</span></span>

## <a name="independent-association"></a><span data-ttu-id="8aa34-135">Associazione indipendente</span><span class="sxs-lookup"><span data-stu-id="8aa34-135">Independent association</span></span>
<span data-ttu-id="8aa34-136">Associazione tra entità in cui non è presente alcuna proprietà che rappresenta la chiave esterna nella classe dell'entità dipendente.</span><span class="sxs-lookup"><span data-stu-id="8aa34-136">An association between entities where there is no property representing the foreign key in the class of the dependent entity.</span></span> <span data-ttu-id="8aa34-137">Una classe prodotto, ad esempio, contiene una relazione con una categoria, ma nessuna proprietà CategoryId.</span><span class="sxs-lookup"><span data-stu-id="8aa34-137">For example, a Product class contains a relationship to Category but no CategoryId property.</span></span> <span data-ttu-id="8aa34-138">Entity Framework tiene traccia dello stato dell'associazione indipendentemente dallo stato delle entità alle due estremità dell'associazione.</span><span class="sxs-lookup"><span data-stu-id="8aa34-138">Entity Framework tracks the state of the association independently of the state of the entities at the two association ends.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="8aa34-139">Caricamento lazy</span><span class="sxs-lookup"><span data-stu-id="8aa34-139">Lazy loading</span></span>
<span data-ttu-id="8aa34-140">Modello di caricamento di dati correlati in cui gli oggetti correlati vengono caricati automaticamente quando si accede a una proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="8aa34-140">A pattern of loading related data where related objects are automatically loaded when a navigation property is accessed.</span></span>

## <a name="model-first"></a><span data-ttu-id="8aa34-141">Model First</span><span class="sxs-lookup"><span data-stu-id="8aa34-141">Model First</span></span>
<span data-ttu-id="8aa34-142">Creazione di un modello di Entity Framework, usando la finestra di progettazione EF, che viene quindi usato per creare un nuovo database.</span><span class="sxs-lookup"><span data-stu-id="8aa34-142">Creating an Entity Framework model, using the EF Designer, that is then used to create a new database.</span></span>

## <a name="navigation-property"></a><span data-ttu-id="8aa34-143">Proprietà di navigazione</span><span class="sxs-lookup"><span data-stu-id="8aa34-143">Navigation property</span></span>
<span data-ttu-id="8aa34-144">Proprietà di un'entità che fa riferimento a un'altra entità.</span><span class="sxs-lookup"><span data-stu-id="8aa34-144">A property of an entity that references another entity.</span></span> <span data-ttu-id="8aa34-145">Il prodotto, ad esempio, contiene una proprietà di navigazione di categoria e la categoria contiene una proprietà di navigazione Products.</span><span class="sxs-lookup"><span data-stu-id="8aa34-145">For example, Product contains a Category navigation property and Category contains a Products navigation property.</span></span>

## <a name="poco"></a><span data-ttu-id="8aa34-146">POCO</span><span class="sxs-lookup"><span data-stu-id="8aa34-146">POCO</span></span>
<span data-ttu-id="8aa34-147">Acronimo dell'oggetto CLR normale.</span><span class="sxs-lookup"><span data-stu-id="8aa34-147">Acronym for Plain-Old CLR Object.</span></span> <span data-ttu-id="8aa34-148">Una semplice classe utente che non ha dipendenze con alcun Framework.</span><span class="sxs-lookup"><span data-stu-id="8aa34-148">A simple user class that has no dependencies with any framework.</span></span> <span data-ttu-id="8aa34-149">Nel contesto di EF, una classe di entità che non deriva da EntityObject, implementa qualsiasi interfaccia o contiene attributi definiti in EF.</span><span class="sxs-lookup"><span data-stu-id="8aa34-149">In the context of EF, an entity class that does not derive from EntityObject, implements any interfaces or carries any attributes defined in EF.</span></span> <span data-ttu-id="8aa34-150">Anche le classi di entità separate dal framework di persistenza sono definite "persistenza ignorata".</span><span class="sxs-lookup"><span data-stu-id="8aa34-150">Such entity classes that are decoupled from the persistence framework are also said to be "persistence ignorant".</span></span>  

## <a name="relationship-inverse"></a><span data-ttu-id="8aa34-151">Relazione inversa</span><span class="sxs-lookup"><span data-stu-id="8aa34-151">Relationship inverse</span></span>
<span data-ttu-id="8aa34-152">Estremità opposta di una relazione, ad esempio Product. Categoria e categoria. Prodotto.</span><span class="sxs-lookup"><span data-stu-id="8aa34-152">The opposite end of a relationship, for example, product.Category and category.Product.</span></span>

## <a name="self-tracking-entity"></a><span data-ttu-id="8aa34-153">Entità con rilevamento automatico</span><span class="sxs-lookup"><span data-stu-id="8aa34-153">Self-tracking entity</span></span>
<span data-ttu-id="8aa34-154">Entità compilata da un modello di generazione del codice che facilita lo sviluppo a più livelli.</span><span class="sxs-lookup"><span data-stu-id="8aa34-154">An entity built from a code generation template that helps with N-Tier development.</span></span>

## <a name="table-per-concrete-type-tpc"></a><span data-ttu-id="8aa34-155">Tipo tabella per calcestruzzo (TPC)</span><span class="sxs-lookup"><span data-stu-id="8aa34-155">Table-per-concrete type (TPC)</span></span>
<span data-ttu-id="8aa34-156">Metodo di mapping dell'ereditarietà in cui ogni tipo non astratto nella gerarchia viene mappato a una tabella separata nel database.</span><span class="sxs-lookup"><span data-stu-id="8aa34-156">A method of mapping the inheritance where each non-abstract type in the hierarchy is mapped to separate table in the database.</span></span>

## <a name="table-per-hierarchy-tph"></a><span data-ttu-id="8aa34-157">Tabella per gerarchia (TPH)</span><span class="sxs-lookup"><span data-stu-id="8aa34-157">Table-per-hierarchy (TPH)</span></span>
<span data-ttu-id="8aa34-158">Metodo di mapping dell'ereditarietà in cui viene eseguito il mapping di tutti i tipi nella gerarchia alla stessa tabella nel database.</span><span class="sxs-lookup"><span data-stu-id="8aa34-158">A method of mapping the inheritance where all types in the hierarchy are mapped to the same table in the database.</span></span> <span data-ttu-id="8aa34-159">Per identificare il tipo a cui è associata ogni riga, viene utilizzata una o più colonne discriminatore.</span><span class="sxs-lookup"><span data-stu-id="8aa34-159">A discriminator column(s) is used to identify what type each row is associated with.</span></span>

## <a name="table-per-type-tpt"></a><span data-ttu-id="8aa34-160">Tabella per tipo (TPT)</span><span class="sxs-lookup"><span data-stu-id="8aa34-160">Table-per-type (TPT)</span></span>
<span data-ttu-id="8aa34-161">Metodo di mapping dell'ereditarietà in cui viene eseguito il mapping delle proprietà comuni di tutti i tipi nella gerarchia alla stessa tabella nel database, ma le proprietà univoche per ogni tipo vengono mappate a una tabella separata.</span><span class="sxs-lookup"><span data-stu-id="8aa34-161">A method of mapping the inheritance where the common properties of all types in the hierarchy are mapped to the same table in the database, but properties unique to each type are mapped to a separate table.</span></span>

## <a name="type-discovery"></a><span data-ttu-id="8aa34-162">Individuazione del tipo</span><span class="sxs-lookup"><span data-stu-id="8aa34-162">Type discovery</span></span>
<span data-ttu-id="8aa34-163">Processo di identificazione dei tipi che devono far parte di un modello di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="8aa34-163">The process of identifying the types that should be part of an Entity Framework model.</span></span>
