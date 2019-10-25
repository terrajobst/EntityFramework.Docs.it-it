---
title: Servizi in fase di progettazione-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: ff0243a588d5e957aed89fcf1ce7462b5b9a747a
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811945"
---
# <a name="design-time-services"></a><span data-ttu-id="f0747-102">Servizi in fase di progettazione</span><span class="sxs-lookup"><span data-stu-id="f0747-102">Design-time services</span></span>

<span data-ttu-id="f0747-103">Alcuni servizi utilizzati dagli strumenti vengono utilizzati solo in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="f0747-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="f0747-104">Questi servizi vengono gestiti separatamente dai servizi di runtime di EF Core per impedire che vengano distribuiti con l'app.</span><span class="sxs-lookup"><span data-stu-id="f0747-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="f0747-105">Per eseguire l'override di uno di questi servizi, ad esempio il servizio per generare file di migrazione, aggiungere un'implementazione di `IDesignTimeServices` al progetto di avvio.</span><span class="sxs-lookup"><span data-stu-id="f0747-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
