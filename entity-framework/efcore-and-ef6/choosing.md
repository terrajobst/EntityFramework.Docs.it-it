---
title: Come scegliere tra EF Core ed EF6
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 9a113e0965fa75a03510199fb75165f6e9be0bbd
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a><span data-ttu-id="8ad6d-102">Come scegliere tra EF Core ed EF6</span><span class="sxs-lookup"><span data-stu-id="8ad6d-102">EF Core and EF6: Which One Is Right for You</span></span>

<span data-ttu-id="8ad6d-103">Le informazioni seguenti consentono di scegliere tra Entity Framework Core ed Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="8ad6d-103">The following information will help you choose between Entity Framework Core and Entity Framework 6.</span></span>

## <a name="guidance-for-new-applications"></a><span data-ttu-id="8ad6d-104">Linee guida per le nuove applicazioni</span><span class="sxs-lookup"><span data-stu-id="8ad6d-104">Guidance for new applications</span></span>

<span data-ttu-id="8ad6d-105">Poiché EF Core è un prodotto nuovo ancora privo di alcune funzionalità O/RM critiche, EF6 resta per il momento la scelta più appropriata per diverse applicazioni.</span><span class="sxs-lookup"><span data-stu-id="8ad6d-105">Because EF Core is a new product, and still lacks some critical O/RM features, EF6 will still be the most suitable choice for many applications.</span></span>

<span data-ttu-id="8ad6d-106">**Questi sono i tipi di applicazioni per cui è consigliabile usare EF Core:**</span><span class="sxs-lookup"><span data-stu-id="8ad6d-106">**These are the types of applications we would recommend using EF Core for:**</span></span>

* <span data-ttu-id="8ad6d-107">Nuove applicazioni che non richiedono funzionalità non ancora implementate in EF Core.</span><span class="sxs-lookup"><span data-stu-id="8ad6d-107">New applications that do not require features that are not yet implemented in EF Core.</span></span> <span data-ttu-id="8ad6d-108">Per verificare se EF Core può essere una scelta adatta per la propria applicazione, vedere [Confronto delle funzionalità](features.md).</span><span class="sxs-lookup"><span data-stu-id="8ad6d-108">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

* <span data-ttu-id="8ad6d-109">Applicazioni per .NET Core, ad esempio le applicazioni UWP (Universal Windows Platform) e ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8ad6d-109">Applications that target .NET Core, such as Universal Windows Platform (UWP) and ASP.NET Core applications.</span></span> <span data-ttu-id="8ad6d-110">Queste applicazioni non possono usare EF6 perché è necessario .NET Framework (ad esempio, .NET Framework 4.5).</span><span class="sxs-lookup"><span data-stu-id="8ad6d-110">These applications can not use EF6 as it requires the .NET Framework (i.e. .NET Framework 4.5).</span></span>

## <a name="guidance-for-existing-ef6-applications"></a><span data-ttu-id="8ad6d-111">Linee guida per le applicazioni EF6 esistenti</span><span class="sxs-lookup"><span data-stu-id="8ad6d-111">Guidance for existing EF6 applications</span></span>

<span data-ttu-id="8ad6d-112">A seguito delle modifiche sostanziali in EF Core, non è consigliabile tentare di spostare un'applicazione da EF6 a EF Core se non in presenza di un valido motivo per apportare la modifica.</span><span class="sxs-lookup"><span data-stu-id="8ad6d-112">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span> <span data-ttu-id="8ad6d-113">Se si vuole passare a EF Core per usare le nuove funzionalità, assicurarsi di essere a conoscenza delle relative limitazioni prima di iniziare.</span><span class="sxs-lookup"><span data-stu-id="8ad6d-113">If you want to move to EF Core to make use of new features, then make sure you are aware of its limitations before you start.</span></span> <span data-ttu-id="8ad6d-114">Per verificare se EF Core può essere una scelta adatta per la propria applicazione, vedere [Confronto delle funzionalità](features.md).</span><span class="sxs-lookup"><span data-stu-id="8ad6d-114">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

<span data-ttu-id="8ad6d-115">**Il trasferimento da EF6 a EF Core deve essere visto come una conversione più che come un aggiornamento.**</span><span class="sxs-lookup"><span data-stu-id="8ad6d-115">**You should view the move from EF6 to EF Core as a port rather than an upgrade.**</span></span> <span data-ttu-id="8ad6d-116">Per altre informazioni, vedere [Porting from EF6 to EF Core](porting/index.md) (Conversione da EF6 a EF Core).</span><span class="sxs-lookup"><span data-stu-id="8ad6d-116">See [Porting from EF6 to EF Core](porting/index.md) for more information.</span></span>
