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
<a name="design-time-services"></a>Servizi in fase di progettazione
====================
Alcuni servizi utilizzati dagli strumenti usati solo in fase di progettazione. Questi servizi vengono gestiti separatamente dai servizi di runtime di EF Core per impedire che vengano distribuite con l'app. Per eseguire l'override di uno di questi servizi (ad esempio il servizio per generare i file di migrazione), aggiungere un'implementazione di `IDesignTimeServices` al progetto di avvio.

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
