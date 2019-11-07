---
title: Schema predefinito-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 1579fed007997aa4cf49b4c1290aee86c81c0000
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655975"
---
# <a name="default-schema"></a><span data-ttu-id="3f8eb-102">Schema predefinito</span><span class="sxs-lookup"><span data-stu-id="3f8eb-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="3f8eb-103">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="3f8eb-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="3f8eb-104">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="3f8eb-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="3f8eb-105">Lo schema predefinito è lo schema del database in cui verranno creati gli oggetti se uno schema non è configurato in modo esplicito per tale oggetto.</span><span class="sxs-lookup"><span data-stu-id="3f8eb-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="3f8eb-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="3f8eb-106">Conventions</span></span>

<span data-ttu-id="3f8eb-107">Per convenzione, il provider di database sceglierà lo schema predefinito più appropriato.</span><span class="sxs-lookup"><span data-stu-id="3f8eb-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="3f8eb-108">Ad esempio, Microsoft SQL Server utilizzerà lo schema `dbo` e SQLite non utilizzerà uno schema (poiché gli schemi non sono supportati in SQLite).</span><span class="sxs-lookup"><span data-stu-id="3f8eb-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="3f8eb-109">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="3f8eb-109">Data Annotations</span></span>

<span data-ttu-id="3f8eb-110">Non è possibile impostare lo schema predefinito utilizzando le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="3f8eb-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="3f8eb-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="3f8eb-111">Fluent API</span></span>

<span data-ttu-id="3f8eb-112">Per specificare uno schema predefinito, è possibile usare l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="3f8eb-112">You can use the Fluent API to specify a default schema.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultSchema.cs?name=DefaultSchema&highlight=7)]
