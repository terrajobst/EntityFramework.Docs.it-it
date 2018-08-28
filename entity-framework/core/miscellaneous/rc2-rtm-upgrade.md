---
title: L'aggiornamento da EF Core 1.0 RC2 a RTM - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 1b95b2ab1943dfb541b3a7c873cff3cb4c16d9c1
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998319"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a><span data-ttu-id="7d69f-102">L'aggiornamento da EF Core 1.0 RC2 a RTM</span><span class="sxs-lookup"><span data-stu-id="7d69f-102">Upgrading from EF Core 1.0 RC2 to RTM</span></span>

<span data-ttu-id="7d69f-103">Questo articolo fornisce indicazioni per lo spostamento di un'applicazione compilata con i pacchetti RC2 1.0.0 RTM.</span><span class="sxs-lookup"><span data-stu-id="7d69f-103">This article provides guidance for moving an application built with the RC2 packages to 1.0.0 RTM.</span></span>

## <a name="package-versions"></a><span data-ttu-id="7d69f-104">Versioni dei pacchetti</span><span class="sxs-lookup"><span data-stu-id="7d69f-104">Package Versions</span></span>

<span data-ttu-id="7d69f-105">I nomi dei pacchetti di livello superiore che viene in genere installato in un'applicazione non sono cambiati tra RC2 e RTM.</span><span class="sxs-lookup"><span data-stu-id="7d69f-105">The names of the top level packages that you would typically install into an application did not change between RC2 and RTM.</span></span>

<span data-ttu-id="7d69f-106">**È necessario aggiornare i pacchetti installati con le versioni RTM:**</span><span class="sxs-lookup"><span data-stu-id="7d69f-106">**You need to upgrade the installed packages to the RTM versions:**</span></span>

* <span data-ttu-id="7d69f-107">Pacchetti di runtime (ad esempio, `Microsoft.EntityFrameworkCore.SqlServer`) modificata `1.0.0-rc2-final` a `1.0.0`.</span><span class="sxs-lookup"><span data-stu-id="7d69f-107">Runtime packages (for example, `Microsoft.EntityFrameworkCore.SqlServer`) changed from `1.0.0-rc2-final` to `1.0.0`.</span></span>

* <span data-ttu-id="7d69f-108">Il `Microsoft.EntityFrameworkCore.Tools` pacchetto modificata `1.0.0-preview1-final` a `1.0.0-preview2-final`.</span><span class="sxs-lookup"><span data-stu-id="7d69f-108">The `Microsoft.EntityFrameworkCore.Tools` package changed from `1.0.0-preview1-final` to `1.0.0-preview2-final`.</span></span> <span data-ttu-id="7d69f-109">Si noti che gli strumenti sono ancora versioni non definitive.</span><span class="sxs-lookup"><span data-stu-id="7d69f-109">Note that tooling is still pre-release.</span></span>

## <a name="existing-migrations-may-need-maxlength-added"></a><span data-ttu-id="7d69f-110">Le migrazioni esistenti potrebbe essere necessario maxLength aggiunto</span><span class="sxs-lookup"><span data-stu-id="7d69f-110">Existing migrations may need maxLength added</span></span>

<span data-ttu-id="7d69f-111">In RC2, era simile alla definizione di colonna in una migrazione `table.Column<string>(nullable: true)` e la lunghezza della colonna è stata cercata in alcuni metadati vengono archiviati nel code-behind della migrazione.</span><span class="sxs-lookup"><span data-stu-id="7d69f-111">In RC2, the column definition in a migration looked like `table.Column<string>(nullable: true)` and the length of the column was looked up in some metadata we store in the code behind the migration.</span></span> <span data-ttu-id="7d69f-112">Nella versione RTM, la lunghezza è ora inclusa nel codice con scaffolding `table.Column<string>(maxLength: 450, nullable: true)`.</span><span class="sxs-lookup"><span data-stu-id="7d69f-112">In RTM, the length is now included in the scaffolded code `table.Column<string>(maxLength: 450, nullable: true)`.</span></span>

<span data-ttu-id="7d69f-113">Le migrazioni esistenti che sono stati sottoposto a scaffolding prima di utilizzare RTM non avranno il `maxLength` argomento specificato.</span><span class="sxs-lookup"><span data-stu-id="7d69f-113">Any existing migrations that were scaffolded prior to using RTM will not have the `maxLength` argument specified.</span></span> <span data-ttu-id="7d69f-114">Ciò significa che la lunghezza massima supportata dal database verrà utilizzata (`nvarchar(max)` su SQL Server).</span><span class="sxs-lookup"><span data-stu-id="7d69f-114">This means the maximum length supported by the database will be used (`nvarchar(max)` on SQL Server).</span></span> <span data-ttu-id="7d69f-115">Ciò potrebbe essere appropriato per alcune colonne, ma le colonne che fanno parte di una chiave, chiave esterna, o devono essere aggiornati per includere una lunghezza massima dell'indice.</span><span class="sxs-lookup"><span data-stu-id="7d69f-115">This may be fine for some columns, but columns that are part of a key, foreign key, or index need to be updated to include a maximum length.</span></span> <span data-ttu-id="7d69f-116">Per convenzione, 450 è la lunghezza massima usata per le chiavi, chiavi esterne e colonne indicizzate.</span><span class="sxs-lookup"><span data-stu-id="7d69f-116">By convention, 450 is the maximum length used for keys, foreign keys, and indexed columns.</span></span> <span data-ttu-id="7d69f-117">Se è stato configurato in modo esplicito una lunghezza nel modello, quindi è necessario utilizzare tale lunghezza.</span><span class="sxs-lookup"><span data-stu-id="7d69f-117">If you have explicitly configured a length in the model, then you should use that length instead.</span></span>

<span data-ttu-id="7d69f-118">**ASP.NET Identity**</span><span class="sxs-lookup"><span data-stu-id="7d69f-118">**ASP.NET Identity**</span></span>

<span data-ttu-id="7d69f-119">Questa modifica interessa i progetti che utilizzano ASP.NET Identity e sono stati creati da una pre-RTM modello di progetto.</span><span class="sxs-lookup"><span data-stu-id="7d69f-119">This change impacts projects that use ASP.NET Identity and were created from a pre-RTM project template.</span></span> <span data-ttu-id="7d69f-120">Il modello di progetto include una migrazione considerata di creare il database.</span><span class="sxs-lookup"><span data-stu-id="7d69f-120">The project template includes a migration used to create the database.</span></span> <span data-ttu-id="7d69f-121">Questa migrazione deve essere modificata per specificare una lunghezza massima di `256` per le colonne seguenti.</span><span class="sxs-lookup"><span data-stu-id="7d69f-121">This migration must be edited to specify a maximum length of `256` for the following columns.</span></span>

*  <span data-ttu-id="7d69f-122">**AspNetRoles**</span><span class="sxs-lookup"><span data-stu-id="7d69f-122">**AspNetRoles**</span></span>

    * <span data-ttu-id="7d69f-123">nome</span><span class="sxs-lookup"><span data-stu-id="7d69f-123">Name</span></span>

    * <span data-ttu-id="7d69f-124">NormalizedName</span><span class="sxs-lookup"><span data-stu-id="7d69f-124">NormalizedName</span></span>

*  <span data-ttu-id="7d69f-125">**AspNetUsers**</span><span class="sxs-lookup"><span data-stu-id="7d69f-125">**AspNetUsers**</span></span>

   * <span data-ttu-id="7d69f-126">Email</span><span class="sxs-lookup"><span data-stu-id="7d69f-126">Email</span></span>

   * <span data-ttu-id="7d69f-127">NormalizedEmail</span><span class="sxs-lookup"><span data-stu-id="7d69f-127">NormalizedEmail</span></span>

   * <span data-ttu-id="7d69f-128">NormalizedUserName</span><span class="sxs-lookup"><span data-stu-id="7d69f-128">NormalizedUserName</span></span>

   * <span data-ttu-id="7d69f-129">UserName</span><span class="sxs-lookup"><span data-stu-id="7d69f-129">UserName</span></span>

<span data-ttu-id="7d69f-130">Errore per apportare questa modifica comporterà l'eccezione seguente durante la migrazione iniziale viene applicata a un database.</span><span class="sxs-lookup"><span data-stu-id="7d69f-130">Failure to make this change will result in the following exception when the initial migration is applied to a database.</span></span>

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a><span data-ttu-id="7d69f-131">.NET core: Rimuovere "imports" nel file Project. JSON</span><span class="sxs-lookup"><span data-stu-id="7d69f-131">.NET Core: Remove "imports" in project.json</span></span>

<span data-ttu-id="7d69f-132">Se si era destinati a .NET Core con RC2, è necessario aggiungere `imports` a Project. JSON come soluzione alternativa temporanea per alcune delle dipendenze di EF Core non supporta .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="7d69f-132">If you were targeting .NET Core with RC2, you needed to add `imports` to project.json as a temporary workaround for some of EF Core's dependencies not supporting .NET Standard.</span></span> <span data-ttu-id="7d69f-133">Questi possono a questo punto verranno rimossi.</span><span class="sxs-lookup"><span data-stu-id="7d69f-133">These can now be removed.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

> [!NOTE]  
> <span data-ttu-id="7d69f-134">A partire dalla versione 1.0 RTM, la [.NET Core SDK](https://www.microsoft.com/net/download/core) non supporta più `project.json` o lo sviluppo di applicazioni .NET Core usando Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="7d69f-134">As of version 1.0 RTM, the [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or developing .NET Core applications using Visual Studio 2015.</span></span> <span data-ttu-id="7d69f-135">È consigliabile [eseguire la migrazione da project.json a csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span><span class="sxs-lookup"><span data-stu-id="7d69f-135">We recommend you [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span></span> <span data-ttu-id="7d69f-136">Se si usa Visual Studio, è consigliabile eseguire l'aggiornamento a [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="7d69f-136">If you are using Visual Studio, we recommend you upgrade to [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

## <a name="uwp-add-binding-redirects"></a><span data-ttu-id="7d69f-137">Piattaforma UWP: Aggiungere reindirizzamenti di associazione</span><span class="sxs-lookup"><span data-stu-id="7d69f-137">UWP: Add binding redirects</span></span>

<span data-ttu-id="7d69f-138">È stato effettuato un tentativo di eseguire comandi EF sui risultati dei progetti di Universal Windows Platform (UWP) l'errore seguente:</span><span class="sxs-lookup"><span data-stu-id="7d69f-138">Attempting to run EF commands on Universal Windows Platform (UWP) projects results in the following error:</span></span>

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

<span data-ttu-id="7d69f-139">È necessario aggiungere manualmente i reindirizzamenti di associazione per il progetto UWP.</span><span class="sxs-lookup"><span data-stu-id="7d69f-139">You need to manually add binding redirects to the UWP project.</span></span> <span data-ttu-id="7d69f-140">Creare un file denominato `App.config` nel progetto di cartella radice e aggiungere reindirizzamenti per le versioni degli assembly corretto.</span><span class="sxs-lookup"><span data-stu-id="7d69f-140">Create a file named `App.config` in the project root folder and add redirects to the correct assembly versions.</span></span>

``` xml
<configuration>
 <runtime>
   <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
     <dependentAssembly>
       <assemblyIdentity name="System.IO.FileSystem.Primitives"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
     <dependentAssembly>
       <assemblyIdentity name="System.Threading.Overlapped"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
   </assemblyBinding>
 </runtime>
</configuration>
```
