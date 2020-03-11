---
title: Selezione di Entity Framework versione runtime per i modelli di progettazione EF-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 40ad05c1f015e6a51150d04eee8d9aa581d72c33
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418147"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a><span data-ttu-id="ad4e6-102">Selezione di Entity Framework versione runtime per i modelli di progettazione EF</span><span class="sxs-lookup"><span data-stu-id="ad4e6-102">Selecting Entity Framework Runtime Version for EF Designer Models</span></span>
> [!NOTE]
> <span data-ttu-id="ad4e6-103">**Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="ad4e6-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="ad4e6-104">Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.</span><span class="sxs-lookup"><span data-stu-id="ad4e6-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="ad4e6-105">A partire da EF6, la schermata seguente è stata aggiunta alla finestra di progettazione EF per consentire di selezionare la versione del runtime di destinazione durante la creazione di un modello.</span><span class="sxs-lookup"><span data-stu-id="ad4e6-105">Starting with EF6 the following screen was added to the EF Designer to allow you to select the version of the runtime you wish to target when creating a model.</span></span> <span data-ttu-id="ad4e6-106">La schermata verrà visualizzata quando la versione più recente di Entity Framework non è ancora installata nel progetto.</span><span class="sxs-lookup"><span data-stu-id="ad4e6-106">The screen will appear when the latest version of Entity Framework is not already installed in the project.</span></span> <span data-ttu-id="ad4e6-107">Se la versione più recente è già installata, verrà usata per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="ad4e6-107">If the latest version is already installed it will just be used by default.</span></span>

![Schermata](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a><span data-ttu-id="ad4e6-109">Destinazione di EF6. x</span><span class="sxs-lookup"><span data-stu-id="ad4e6-109">Targeting EF6.x</span></span>

<span data-ttu-id="ad4e6-110">È possibile scegliere EF6 dalla schermata ' scegliere la versione ' per aggiungere il runtime di EF6 al progetto.</span><span class="sxs-lookup"><span data-stu-id="ad4e6-110">You can choose EF6 from the 'Choose Your Version' screen to add the EF6 runtime to your project.</span></span> <span data-ttu-id="ad4e6-111">Una volta aggiunto EF6, questa schermata verrà arrestata nel progetto corrente.</span><span class="sxs-lookup"><span data-stu-id="ad4e6-111">Once you've added EF6, you’ll stop seeing this screen in the current project.</span></span>

<span data-ttu-id="ad4e6-112">EF6 verrà disabilitato se è già installata una versione precedente di EF (poiché non è possibile fare riferimento a più versioni del runtime dallo stesso progetto).</span><span class="sxs-lookup"><span data-stu-id="ad4e6-112">EF6 will be disabled if you already have an older version of EF installed (since you can't target multiple versions of the runtime from the same project).</span></span> <span data-ttu-id="ad4e6-113">Se l'opzione EF6 non è abilitata, seguire questa procedura per aggiornare il progetto a EF6:</span><span class="sxs-lookup"><span data-stu-id="ad4e6-113">If EF6 option is not enabled here, follow these steps to upgrade your project to EF6:</span></span>

1.  <span data-ttu-id="ad4e6-114">Fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni e scegliere **Gestisci pacchetti NuGet...**</span><span class="sxs-lookup"><span data-stu-id="ad4e6-114">Right-click on your project in Solution Explorer and select **Manage NuGet Packages...**</span></span>
2.  <span data-ttu-id="ad4e6-115">Seleziona **aggiornamenti**</span><span class="sxs-lookup"><span data-stu-id="ad4e6-115">Select **Updates**</span></span>
3.  <span data-ttu-id="ad4e6-116">Selezionare **EntityFramework** (assicurarsi che sia in corso l'aggiornamento alla versione desiderata)</span><span class="sxs-lookup"><span data-stu-id="ad4e6-116">Select **EntityFramework** (make sure it is going to update it to the version you want)</span></span>
4.  <span data-ttu-id="ad4e6-117">Fare clic su **Update** (Aggiorna).</span><span class="sxs-lookup"><span data-stu-id="ad4e6-117">Click **Update**</span></span>

 

## <a name="targeting-ef5x"></a><span data-ttu-id="ad4e6-118">Destinazione di EF5. x</span><span class="sxs-lookup"><span data-stu-id="ad4e6-118">Targeting EF5.x</span></span>

<span data-ttu-id="ad4e6-119">È possibile scegliere EF5 dalla schermata ' scegliere la versione ' per aggiungere il runtime di EF5 al progetto.</span><span class="sxs-lookup"><span data-stu-id="ad4e6-119">You can choose EF5 from the 'Choose Your Version' screen to add the EF5 runtime to your project.</span></span> <span data-ttu-id="ad4e6-120">Una volta aggiunto EF5, viene visualizzata la schermata con l'opzione EF6 disabilitata.</span><span class="sxs-lookup"><span data-stu-id="ad4e6-120">Once you've added EF5, you’ll still see the screen with the EF6 option disabled.</span></span>

<span data-ttu-id="ad4e6-121">Se è già installata una versione EF4. x del runtime, si noterà che la versione di EF è elencata nella schermata invece che in EF5.</span><span class="sxs-lookup"><span data-stu-id="ad4e6-121">If you have an EF4.x version of the runtime already installed then you will see that version of EF listed in the screen rather than EF5.</span></span> <span data-ttu-id="ad4e6-122">In questa situazione è possibile eseguire l'aggiornamento a EF5 attenendosi alla procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="ad4e6-122">In this situation you can upgrade to EF5 using the following steps:</span></span>

1.  <span data-ttu-id="ad4e6-123">Selezione **strumenti-&gt; gestione pacchetti libreria-&gt; console di gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="ad4e6-123">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="ad4e6-124">Eseguire **Install-Package EntityFramework-Version 5.0.0**</span><span class="sxs-lookup"><span data-stu-id="ad4e6-124">Run **Install-Package EntityFramework -version 5.0.0**</span></span>

 

## <a name="targeting-ef4x"></a><span data-ttu-id="ad4e6-125">Destinazione di EF4. x</span><span class="sxs-lookup"><span data-stu-id="ad4e6-125">Targeting EF4.x</span></span>

<span data-ttu-id="ad4e6-126">È possibile installare il runtime EF4. x nel progetto attenendosi alla procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="ad4e6-126">You can install the EF4.x runtime to your project using the following steps:</span></span>

1.  <span data-ttu-id="ad4e6-127">Selezione **strumenti-&gt; gestione pacchetti libreria-&gt; console di gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="ad4e6-127">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="ad4e6-128">Eseguire **Install-Package EntityFramework-Version 4.3.0**</span><span class="sxs-lookup"><span data-stu-id="ad4e6-128">Run **Install-Package EntityFramework -version 4.3.0**</span></span>
