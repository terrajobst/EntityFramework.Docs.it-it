---
title: Come scegliere tra EF Core ed EF6
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 17c81e0d6c384c06e45f0cf4f338d4ba402788e1
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949140"
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a><span data-ttu-id="33311-102">Come scegliere tra EF Core ed EF6</span><span class="sxs-lookup"><span data-stu-id="33311-102">EF Core and EF6: Which One Is Right for You</span></span>

<span data-ttu-id="33311-103">Le informazioni seguenti consentono di scegliere tra Entity Framework Core ed Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="33311-103">The following information will help you choose between Entity Framework Core and Entity Framework 6.</span></span>

## <a name="guidance-for-new-applications"></a><span data-ttu-id="33311-104">Linee guida per le nuove applicazioni</span><span class="sxs-lookup"><span data-stu-id="33311-104">Guidance for new applications</span></span>

<span data-ttu-id="33311-105">Può essere utile usare EF Core per le nuove applicazioni per avere a disposizione tutte le funzionalità di EF Core e quando l'applicazione non richiede funzionalità che non sono state ancora implementate in EF Core.</span><span class="sxs-lookup"><span data-stu-id="33311-105">Consider using EF Core for new applications if you want to take advantage of the all the capabilities of EF Core and your application does not require any features that are not yet implemented in EF Core.</span></span>

<span data-ttu-id="33311-106">EF6 richiede .NET Framework 4.0 o versione successiva ed è supportato solo in Windows (ovvero EF6 non può essere eseguito in .NET Core e non è supportato in altri sistemi operativi), ma rappresenta ancora una valida scelta per le nuove applicazioni a condizione che tali vincoli siano accettabili e l'applicazione non richieda nuove funzionalità in EF Core non disponibili per EF6.</span><span class="sxs-lookup"><span data-stu-id="33311-106">EF6 requires .NET Framework 4.0 (or a later version) and is only supported on Windows (that is, EF6 does not currently run on .NET Core and is not supported in other operating systems), but it is still a viable choice for new applications as long those constraints are acceptable and the application does not require new features in EF Core that are not available to EF6.</span></span>

<span data-ttu-id="33311-107">Per verificare se EF Core può essere una scelta adatta per la propria applicazione, vedere [Confronto delle funzionalità](features.md).</span><span class="sxs-lookup"><span data-stu-id="33311-107">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

## <a name="guidance-for-existing-ef6-applications"></a><span data-ttu-id="33311-108">Linee guida per le applicazioni EF6 esistenti</span><span class="sxs-lookup"><span data-stu-id="33311-108">Guidance for existing EF6 applications</span></span>

<span data-ttu-id="33311-109">A seguito delle modifiche sostanziali in EF Core, non è consigliabile tentare di spostare un'applicazione da EF6 a EF Core se non in presenza di un valido motivo per apportare la modifica.</span><span class="sxs-lookup"><span data-stu-id="33311-109">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span> <span data-ttu-id="33311-110">Se si vuole passare a EF Core per usare le nuove funzionalità, assicurarsi di essere a conoscenza delle relative limitazioni prima di iniziare.</span><span class="sxs-lookup"><span data-stu-id="33311-110">If you want to move to EF Core to make use of new features, then make sure you are aware of its limitations before you start.</span></span> <span data-ttu-id="33311-111">Per verificare se EF Core può essere una scelta adatta per la propria applicazione, vedere [Confronto delle funzionalità](features.md).</span><span class="sxs-lookup"><span data-stu-id="33311-111">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

<span data-ttu-id="33311-112">**Il trasferimento da EF6 a EF Core deve essere visto come una conversione più che come un aggiornamento.**</span><span class="sxs-lookup"><span data-stu-id="33311-112">**You should view the move from EF6 to EF Core as a port rather than an upgrade.**</span></span> <span data-ttu-id="33311-113">Per altre informazioni, vedere [Porting from EF6 to EF Core](porting/index.md) (Conversione da EF6 a EF Core).</span><span class="sxs-lookup"><span data-stu-id="33311-113">See [Porting from EF6 to EF Core](porting/index.md) for more information.</span></span>
