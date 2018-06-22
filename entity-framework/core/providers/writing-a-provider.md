---
title: Scrittura di un Provider di Database - Core a Entity Framework
author: anmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
ms.technology: entity-framework-core
uid: core/providers/writing-a-provider
ms.openlocfilehash: 4bddf5858ab2c6b2fd22571a20edb3f7c85e2853
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678960"
---
# <a name="writing-a-database-provider"></a>Scrittura di un Provider di Database

Per informazioni sulla scrittura di un provider di database di Entity Framework Core, vedere [, pertanto si desidera scrivere un provider di Entity Framework Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) da [Arthur Vickers](https://github.com/ajcvickers).

La base di codice di Entity Framework Core open source e contiene diversi provider di database che può essere usato come riferimento. È possibile trovare il codice sorgente in https://github.com/aspnet/EntityFrameworkCore.

## <a name="the-providers-beware-label"></a>Il provider-beware etichetta

Quando si inizia a lavorare su un provider, cercare il [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) etichetta sui problemi di GitHub e richieste pull. Si può essere utilizzata per identificare le modifiche che potrebbero influire sulla writer di provider.

## <a name="suggested-naming-of-third-party-providers"></a>Suggerito di denominazione dei provider di terze parti

È consigliabile utilizzare i seguenti nomi per i pacchetti NuGet. Ciò è coerente con i nomi dei pacchetti recapitati dal team di EF Core.

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

Ad esempio:
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
