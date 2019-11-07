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
# <a name="design-time-services"></a>Servizi in fase di progettazione

Alcuni servizi utilizzati dagli strumenti vengono utilizzati solo in fase di progettazione. Questi servizi vengono gestiti separatamente dai servizi di runtime di EF Core per impedire che vengano distribuiti con l'app. Per eseguire l'override di uno di questi servizi, ad esempio il servizio per generare file di migrazione, aggiungere un'implementazione di `IDesignTimeServices` al progetto di avvio.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs)]
