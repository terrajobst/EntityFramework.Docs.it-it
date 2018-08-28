---
title: Servizi in fase di progettazione - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
ms.openlocfilehash: e1cacdd4f40f9c395d8c88a91df4a92ef27001a8
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997531"
---
<a name="design-time-services"></a><span data-ttu-id="fcfd2-102">Servizi in fase di progettazione</span><span class="sxs-lookup"><span data-stu-id="fcfd2-102">Design-time services</span></span>
====================
<span data-ttu-id="fcfd2-103">Alcuni servizi utilizzati dagli strumenti usati solo in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="fcfd2-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="fcfd2-104">Questi servizi vengono gestiti separatamente dai servizi di runtime di EF Core per impedire che vengano distribuite con l'app.</span><span class="sxs-lookup"><span data-stu-id="fcfd2-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="fcfd2-105">Per eseguire l'override di uno di questi servizi (ad esempio il servizio per generare i file di migrazione), aggiungere un'implementazione di `IDesignTimeServices` al progetto di avvio.</span><span class="sxs-lookup"><span data-stu-id="fcfd2-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
