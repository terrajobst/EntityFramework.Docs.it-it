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
<a name="design-time-services"></a>Servizi in fase di progettazione
====================
Alcuni servizi utilizzati dagli strumenti di vengono utilizzate solo in fase di progettazione. Questi servizi vengono gestiti separatamente dai servizi di runtime di Entity Framework Core per evitare che venga distribuito con l'app. Per eseguire l'override di uno di questi servizi (ad esempio il servizio per generare i file di migrazione), aggiungere un'implementazione di `IDesignTimeServices` al progetto di avvio.

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
