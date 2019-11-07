---
title: Valori predefiniti-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: b6ac283d551e2c6ee119f7de6933363b5d8793a1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655908"
---
# <a name="default-values"></a><span data-ttu-id="22732-102">Valori predefiniti</span><span class="sxs-lookup"><span data-stu-id="22732-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="22732-103">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="22732-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="22732-104">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="22732-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="22732-105">Il valore predefinito di una colonna è il valore che verrà inserito se viene inserita una nuova riga, ma non viene specificato alcun valore per la colonna.</span><span class="sxs-lookup"><span data-stu-id="22732-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="22732-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="22732-106">Conventions</span></span>

<span data-ttu-id="22732-107">Per convenzione, un valore predefinito non è configurato.</span><span class="sxs-lookup"><span data-stu-id="22732-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="22732-108">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="22732-108">Data Annotations</span></span>

<span data-ttu-id="22732-109">Non è possibile impostare un valore predefinito utilizzando le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="22732-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="22732-110">API Fluent</span><span class="sxs-lookup"><span data-stu-id="22732-110">Fluent API</span></span>

<span data-ttu-id="22732-111">È possibile usare l'API Fluent per specificare il valore predefinito per una proprietà.</span><span class="sxs-lookup"><span data-stu-id="22732-111">You can use the Fluent API to specify the default value for a property.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValue.cs?name=DefaultValue&highlight=9)]

<span data-ttu-id="22732-112">È inoltre possibile specificare un frammento SQL utilizzato per calcolare il valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="22732-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValueSql.cs?name=DefaultValueSql&highlight=9)]
