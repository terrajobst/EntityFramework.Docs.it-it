---
title: Servizi in fase di progettazione-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: 57294ab41e7c251b1dafae9d573aa98676c5d939
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416728"
---
# <a name="design-time-services"></a><span data-ttu-id="1695c-102">Servizi in fase di progettazione</span><span class="sxs-lookup"><span data-stu-id="1695c-102">Design-time services</span></span>

<span data-ttu-id="1695c-103">Alcuni servizi utilizzati dagli strumenti vengono utilizzati solo in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="1695c-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="1695c-104">Questi servizi vengono gestiti separatamente dai servizi di runtime di EF Core per impedire che vengano distribuiti con l'app.</span><span class="sxs-lookup"><span data-stu-id="1695c-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="1695c-105">Per eseguire l'override di uno di questi servizi, ad esempio il servizio per generare file di migrazione, aggiungere un'implementazione di `IDesignTimeServices` al progetto di avvio.</span><span class="sxs-lookup"><span data-stu-id="1695c-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs)]
