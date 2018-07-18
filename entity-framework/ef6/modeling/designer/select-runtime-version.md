---
title: Se si seleziona versione di Runtime di Entity Framework per i modelli della finestra di progettazione di Entity Framework - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
caps.latest.revision: 3
ms.openlocfilehash: 75f7b4ed81528683801893c31de490ce15be6733
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121364"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a><span data-ttu-id="bead4-102">Se si seleziona versione di Runtime di Entity Framework per i modelli di progettazione di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="bead4-102">Selecting Entity Framework Runtime Version for EF Designer Models</span></span>
> [!NOTE]
> <span data-ttu-id="bead4-103">**Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="bead4-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="bead4-104">Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.</span><span class="sxs-lookup"><span data-stu-id="bead4-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="bead4-105">A partire da Entity Framework 6 la schermata seguente è stata aggiunta alla finestra di progettazione di Entity Framework consente di selezionare la versione del runtime di destinazione durante la creazione di un modello.</span><span class="sxs-lookup"><span data-stu-id="bead4-105">Starting with EF6 the following screen was added to the EF Designer to allow you to select the version of the runtime you wish to target when creating a model.</span></span> <span data-ttu-id="bead4-106">Verrà visualizzata la schermata quando la versione più recente di Entity Framework non è già installata nel progetto.</span><span class="sxs-lookup"><span data-stu-id="bead4-106">The screen will appear when the latest version of Entity Framework is not already installed in the project.</span></span> <span data-ttu-id="bead4-107">Se la versione più recente è già installata verrà usato solo per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="bead4-107">If the latest version is already installed it will just be used by default.</span></span>

![Schermata](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a><span data-ttu-id="bead4-109">Targeting EF6.x</span><span class="sxs-lookup"><span data-stu-id="bead4-109">Targeting EF6.x</span></span>

<span data-ttu-id="bead4-110">È possibile scegliere EF6 nella schermata "Scegliere Version" per aggiungere il runtime di Entity Framework 6 per il progetto.</span><span class="sxs-lookup"><span data-stu-id="bead4-110">You can choose EF6 from the 'Choose Your Version' screen to add the EF6 runtime to your project.</span></span> <span data-ttu-id="bead4-111">Dopo aver aggiunto EF6, sarà più visualizzata questa schermata nel progetto corrente.</span><span class="sxs-lookup"><span data-stu-id="bead4-111">Once you've added EF6, you’ll stop seeing this screen in the current project.</span></span>

<span data-ttu-id="bead4-112">EF6 verrà disabilitata se si dispone già di una versione precedente di Entity Framework installato (poiché non è possibile destinazione più versioni del runtime dallo stesso progetto).</span><span class="sxs-lookup"><span data-stu-id="bead4-112">EF6 will be disabled if you already have an older version of EF installed (since you can't target multiple versions of the runtime from the same project).</span></span> <span data-ttu-id="bead4-113">Se l'opzione di Entity Framework 6 non è abilitata in questo caso, seguire questi passaggi per aggiornare il progetto a EF6:</span><span class="sxs-lookup"><span data-stu-id="bead4-113">If EF6 option is not enabled here, follow these steps to upgrade your project to EF6:</span></span>

1.  <span data-ttu-id="bead4-114">Pulsante destro del mouse sul progetto in Esplora soluzioni e selezionare **Gestisci pacchetti NuGet...**</span><span class="sxs-lookup"><span data-stu-id="bead4-114">Right-click on your project in Solution Explorer and select **Manage NuGet Packages...**</span></span>
2.  <span data-ttu-id="bead4-115">Selezionare **aggiornamenti**</span><span class="sxs-lookup"><span data-stu-id="bead4-115">Select **Updates**</span></span>
3.  <span data-ttu-id="bead4-116">Selezionare **EntityFramework** (assicurarsi che sta per aggiornarlo alla versione desiderata)</span><span class="sxs-lookup"><span data-stu-id="bead4-116">Select **EntityFramework** (make sure it is going to update it to the version you want)</span></span>
4.  <span data-ttu-id="bead4-117">Fare clic su **Update**</span><span class="sxs-lookup"><span data-stu-id="bead4-117">Click **Update**</span></span>

 

## <a name="targeting-ef5x"></a><span data-ttu-id="bead4-118">Targeting EF5.x</span><span class="sxs-lookup"><span data-stu-id="bead4-118">Targeting EF5.x</span></span>

<span data-ttu-id="bead4-119">È possibile scegliere EF5 dalla schermata "Scegliere Version" per aggiungere il runtime EF5 al progetto.</span><span class="sxs-lookup"><span data-stu-id="bead4-119">You can choose EF5 from the 'Choose Your Version' screen to add the EF5 runtime to your project.</span></span> <span data-ttu-id="bead4-120">Dopo aver aggiunto EF5, si verrà comunque visualizzata la schermata con l'opzione di EF6 disabilitato.</span><span class="sxs-lookup"><span data-stu-id="bead4-120">Once you've added EF5, you’ll still see the screen with the EF6 option disabled.</span></span>

<span data-ttu-id="bead4-121">Se si dispone di una versione EF4.x del runtime già installata è verrà visualizzato tale versione di Entity Framework elencati nella schermata anziché EF5.</span><span class="sxs-lookup"><span data-stu-id="bead4-121">If you have an EF4.x version of the runtime already installed then you will see that version of EF listed in the screen rather than EF5.</span></span> <span data-ttu-id="bead4-122">In questo caso è possibile aggiornare a EF5 usando la procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="bead4-122">In this situation you can upgrade to EF5 using the following steps:</span></span>

1.  <span data-ttu-id="bead4-123">Selezionare **- Tools&gt; Gestione pacchetti libreria -&gt; Console di gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="bead4-123">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="bead4-124">Eseguire **Install-Package EntityFramework-versione 5.0.0**</span><span class="sxs-lookup"><span data-stu-id="bead4-124">Run **Install-Package EntityFramework -version 5.0.0**</span></span>

 

## <a name="targeting-ef4x"></a><span data-ttu-id="bead4-125">Targeting EF4.x</span><span class="sxs-lookup"><span data-stu-id="bead4-125">Targeting EF4.x</span></span>

<span data-ttu-id="bead4-126">È possibile installare il runtime EF4.x al progetto usando la procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="bead4-126">You can install the EF4.x runtime to your project using the following steps:</span></span>

1.  <span data-ttu-id="bead4-127">Selezionare **- Tools&gt; Gestione pacchetti libreria -&gt; Console di gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="bead4-127">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="bead4-128">Eseguire **Install-Package EntityFramework-versione 4.3.0**</span><span class="sxs-lookup"><span data-stu-id="bead4-128">Run **Install-Package EntityFramework -version 4.3.0**</span></span>
