---
title: Proprietà Shadow-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: ab57358dd247e32c4ca0f57d07b4cb98f2b85d29
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655960"
---
# <a name="shadow-properties"></a><span data-ttu-id="03cc4-102">Proprietà shadow</span><span class="sxs-lookup"><span data-stu-id="03cc4-102">Shadow Properties</span></span>

<span data-ttu-id="03cc4-103">Le proprietà shadow sono proprietà che non sono definite nella classe di entità .NET, ma sono definite per il tipo di entità nel modello di EF Core.</span><span class="sxs-lookup"><span data-stu-id="03cc4-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="03cc4-104">Il valore e lo stato di queste proprietà vengono mantenuti esclusivamente nello strumento di rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="03cc4-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="03cc4-105">Le proprietà shadow sono utili quando nel database sono presenti dati che non devono essere esposti sui tipi di entità mappati.</span><span class="sxs-lookup"><span data-stu-id="03cc4-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="03cc4-106">Vengono spesso usate per le proprietà di chiave esterna, in cui la relazione tra due entità è rappresentata da un valore di chiave esterna nel database, ma la relazione viene gestita sui tipi di entità usando le proprietà di navigazione tra i tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="03cc4-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="03cc4-107">I valori delle proprietà shadow possono essere ottenuti e modificati tramite l'API `ChangeTracker`.</span><span class="sxs-lookup"><span data-stu-id="03cc4-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="03cc4-108">È possibile fare riferimento alle proprietà shadow nelle query LINQ tramite il metodo statico `EF.Property`.</span><span class="sxs-lookup"><span data-stu-id="03cc4-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="03cc4-109">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="03cc4-109">Conventions</span></span>

<span data-ttu-id="03cc4-110">Le proprietà shadow possono essere create per convenzione quando viene individuata una relazione, ma nella classe di entità dipendente non viene trovata alcuna proprietà di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="03cc4-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="03cc4-111">In questo caso verrà introdotta una proprietà di chiave esterna Shadow.</span><span class="sxs-lookup"><span data-stu-id="03cc4-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="03cc4-112">La proprietà della chiave esterna shadow verrà denominata `<navigation property name><principal key property name>` (la navigazione sull'entità dipendente, che fa riferimento all'entità principale, viene utilizzata per la denominazione).</span><span class="sxs-lookup"><span data-stu-id="03cc4-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="03cc4-113">Se il nome della proprietà chiave principale include il nome della proprietà di navigazione, il nome sarà appena `<principal key property name>`.</span><span class="sxs-lookup"><span data-stu-id="03cc4-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="03cc4-114">Se non è presente alcuna proprietà di navigazione nell'entità dipendente, il nome del tipo di entità viene usato al suo posto.</span><span class="sxs-lookup"><span data-stu-id="03cc4-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="03cc4-115">Il seguente listato di codice, ad esempio, comporterà l'introduzione di una proprietà shadow `BlogId` all'entità `Post`.</span><span class="sxs-lookup"><span data-stu-id="03cc4-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions)]

## <a name="data-annotations"></a><span data-ttu-id="03cc4-116">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="03cc4-116">Data Annotations</span></span>

<span data-ttu-id="03cc4-117">Non è possibile creare le proprietà shadow con le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="03cc4-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="03cc4-118">API Fluent</span><span class="sxs-lookup"><span data-stu-id="03cc4-118">Fluent API</span></span>

<span data-ttu-id="03cc4-119">Per configurare le proprietà shadow, è possibile usare l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="03cc4-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="03cc4-120">Una volta chiamato l'overload di stringa di `Property` è possibile concatenare qualsiasi chiamata di configurazione per altre proprietà.</span><span class="sxs-lookup"><span data-stu-id="03cc4-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="03cc4-121">Se il nome fornito al metodo `Property` corrisponde al nome di una proprietà esistente (una proprietà shadow o un oggetto definito nella classe di entità), il codice configurerà la proprietà esistente anziché introdurre una nuova proprietà shadow.</span><span class="sxs-lookup"><span data-stu-id="03cc4-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]
