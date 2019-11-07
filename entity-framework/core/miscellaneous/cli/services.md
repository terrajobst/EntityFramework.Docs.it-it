---
title: Servizi in fase di progettazione-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: 57294ab41e7c251b1dafae9d573aa98676c5d939
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655829"
---
# <a name="design-time-services"></a><span data-ttu-id="14b81-102">Servizi in fase di progettazione</span><span class="sxs-lookup"><span data-stu-id="14b81-102">Design-time services</span></span>

<span data-ttu-id="14b81-103">Alcuni servizi utilizzati dagli strumenti vengono utilizzati solo in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="14b81-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="14b81-104">Questi servizi vengono gestiti separatamente dai servizi di runtime di EF Core per impedire che vengano distribuiti con l'app.</span><span class="sxs-lookup"><span data-stu-id="14b81-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="14b81-105">Per eseguire l'override di uno di questi servizi, ad esempio il servizio per generare file di migrazione, aggiungere un'implementazione di `IDesignTimeServices` al progetto di avvio.</span><span class="sxs-lookup"><span data-stu-id="14b81-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs)]
