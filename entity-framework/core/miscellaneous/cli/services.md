---
title: Servizi in fase di progettazione - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
ms.technology: entity-framework-core
ms.openlocfilehash: f9c8208a59bfcefeaab01ea69e65fe809a0b3d89
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
---
<a name="design-time-services"></a><span data-ttu-id="3f234-102">Servizi in fase di progettazione</span><span class="sxs-lookup"><span data-stu-id="3f234-102">Design-time services</span></span>
====================
<span data-ttu-id="3f234-103">Alcuni servizi utilizzati dagli strumenti di vengono utilizzate solo in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="3f234-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="3f234-104">Questi servizi vengono gestiti separatamente dai servizi di runtime di Entity Framework Core per evitare che venga distribuito con l'app.</span><span class="sxs-lookup"><span data-stu-id="3f234-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="3f234-105">Per eseguire l'override di uno di questi servizi (ad esempio il servizio per generare i file di migrazione), aggiungere un'implementazione di `IDesignTimeServices` al progetto di avvio.</span><span class="sxs-lookup"><span data-stu-id="3f234-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
