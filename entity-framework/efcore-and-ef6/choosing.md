---
title: Come scegliere tra EF Core ed EF6
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: f0a632902384a65ea3cddf752ad262c7a2e89e2e
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2018
ms.locfileid: "30002821"
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a><span data-ttu-id="8b5f5-102">Come scegliere tra EF Core ed EF6</span><span class="sxs-lookup"><span data-stu-id="8b5f5-102">EF Core and EF6: Which One Is Right for You</span></span>

<span data-ttu-id="8b5f5-103">Le informazioni seguenti consentono di scegliere tra Entity Framework Core ed Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="8b5f5-103">The following information will help you choose between Entity Framework Core and Entity Framework 6.</span></span>

## <a name="guidance-for-new-applications"></a><span data-ttu-id="8b5f5-104">Linee guida per le nuove applicazioni</span><span class="sxs-lookup"><span data-stu-id="8b5f5-104">Guidance for new applications</span></span>

<span data-ttu-id="8b5f5-105">Può essere utile usare EF Core per le nuove applicazioni per avere a disposizione tutte le funzionalità di EF Core e quando l'applicazione non richiede funzionalità che non sono state ancora implementate in EF Core.</span><span class="sxs-lookup"><span data-stu-id="8b5f5-105">Consider using EF Core for new applications if you want to take advantage of the all the capabilities of EF Core and your application does not require any features that are not yet implemented in EF Core.</span></span>

<span data-ttu-id="8b5f5-106">EF6 richiede .NET Framework 4.0 o versione successiva ed è supportato solo in Windows (non può essere eseguito in .NET Core e non è supportato in altri sistemi operativi), ma rappresenta ancora una valida scelta per le nuove applicazioni a condizione che tali vincoli siano accettabili e l'applicazione non richieda nuove funzionalità in EF Core non disponibili per EF6.</span><span class="sxs-lookup"><span data-stu-id="8b5f5-106">EF6 requires .NET Framework 4.0 (or a later version) and is only supported on Windows (i.e. it does not run on .NET Core and is not supported in other operating systems), but it is still a viable choice for new applications as long those constraints are acceptable and the application does not require new features in EF Core that are not available to EF6.</span></span>

<span data-ttu-id="8b5f5-107">Per verificare se EF Core può essere una scelta adatta per la propria applicazione, vedere [Confronto delle funzionalità](features.md).</span><span class="sxs-lookup"><span data-stu-id="8b5f5-107">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

## <a name="guidance-for-existing-ef6-applications"></a><span data-ttu-id="8b5f5-108">Linee guida per le applicazioni EF6 esistenti</span><span class="sxs-lookup"><span data-stu-id="8b5f5-108">Guidance for existing EF6 applications</span></span>

<span data-ttu-id="8b5f5-109">A seguito delle modifiche sostanziali in EF Core, non è consigliabile tentare di spostare un'applicazione da EF6 a EF Core se non in presenza di un valido motivo per apportare la modifica.</span><span class="sxs-lookup"><span data-stu-id="8b5f5-109">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span> <span data-ttu-id="8b5f5-110">Se si vuole passare a EF Core per usare le nuove funzionalità, assicurarsi di essere a conoscenza delle relative limitazioni prima di iniziare.</span><span class="sxs-lookup"><span data-stu-id="8b5f5-110">If you want to move to EF Core to make use of new features, then make sure you are aware of its limitations before you start.</span></span> <span data-ttu-id="8b5f5-111">Per verificare se EF Core può essere una scelta adatta per la propria applicazione, vedere [Confronto delle funzionalità](features.md).</span><span class="sxs-lookup"><span data-stu-id="8b5f5-111">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

<span data-ttu-id="8b5f5-112">**Il trasferimento da EF6 a EF Core deve essere visto come una conversione più che come un aggiornamento.**</span><span class="sxs-lookup"><span data-stu-id="8b5f5-112">**You should view the move from EF6 to EF Core as a port rather than an upgrade.**</span></span> <span data-ttu-id="8b5f5-113">Per altre informazioni, vedere [Porting from EF6 to EF Core](porting/index.md) (Conversione da EF6 a EF Core).</span><span class="sxs-lookup"><span data-stu-id="8b5f5-113">See [Porting from EF6 to EF Core](porting/index.md) for more information.</span></span>
