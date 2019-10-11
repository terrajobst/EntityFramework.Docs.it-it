---
title: Supporto del provider per i tipi spaziali-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: 863f1b4551bd62160915eba90fee7ba6c49c169c
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181601"
---
# <a name="provider-support-for-spatial-types"></a>Supporto del provider per i tipi spaziali
Entity Framework supporta l'utilizzo di dati spaziali tramite le classi DbGeography o DbGeometry. Queste classi si basano sulle funzionalità specifiche del database offerte dal provider Entity Framework. Non tutti i provider supportano i dati spaziali e quelli che possono avere prerequisiti aggiuntivi, ad esempio l'installazione di assembly di tipo spaziale. Altre informazioni sul supporto dei provider per i tipi spaziali sono disponibili di seguito.  

Altre informazioni su come usare i tipi spaziali in un'applicazione sono disponibili in due procedure dettagliate, una per Code First, l'altra per Database First o Model First:  

- [Tipi di dati spaziali in Code First](~/ef6/modeling/code-first/data-types/spatial.md)  
- [Tipi di dati spaziali in EF designer](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a>Versioni EF che supportano i tipi spaziali  

Il supporto per i tipi spaziali è stato introdotto in EF5. Tuttavia, nei tipi spaziali EF5 sono supportati solo quando l'applicazione è destinata ed è in esecuzione in .NET 4,5.  

A partire da i tipi spaziali EF6 sono supportati per le applicazioni destinate sia a .NET 4 che a .NET 4,5.  

## <a name="ef-providers-that-support-spatial-types"></a>Provider EF che supportano i tipi spaziali  

### <a name="ef5"></a>EF5  

I provider Entity Framework per EF5 che sono in grado di supportare i tipi spaziali sono:  

- Provider di Microsoft SQL Server  
    - Questo provider viene fornito come parte di EF5.  
    - Questo provider dipende da alcune librerie di basso livello aggiuntive che potrebbero dover essere installate. per informazioni dettagliate, vedere di seguito.  
- [Devart dotConnect per Oracle](https://www.devart.com/dotconnect/oracle/)  
    - Si tratta di un provider di terze parti di Devart.  

Se si è a conoscenza di un provider EF5 che supporta i tipi spaziali, contattare il contatto e sarà possibile aggiungerlo a questo elenco.  

### <a name="ef6"></a>EF6  

I provider Entity Framework per EF6 che sono in grado di supportare i tipi spaziali sono:  

- Provider di Microsoft SQL Server  
    - Questo provider viene fornito come parte di EF6.  
    - Questo provider dipende da alcune librerie di basso livello aggiuntive che potrebbero dover essere installate. per informazioni dettagliate, vedere di seguito.  
- [Devart dotConnect per Oracle](https://www.devart.com/dotconnect/oracle/)  
    - Si tratta di un provider di terze parti di Devart.  

Se si è a conoscenza di un provider EF6 che supporta i tipi spaziali, contattare il contatto e sarà possibile aggiungerlo a questo elenco.  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a>Prerequisiti per i tipi spaziali con Microsoft SQL Server  

SQL Server supporto spaziale dipende dai tipi SqlGeography e SqlGeometry di basso livello SQL Server specifici. Questi tipi risiedono nell'assembly Microsoft. SqlServer. Types. dll e questo assembly non viene fornito come parte di EF o come parte del .NET Framework.  

Quando si installa Visual Studio, viene spesso installata anche una versione di SQL Server, che includerà l'installazione di Microsoft. SqlServer. Types. dll.  

Se SQL Server non è installato nel computer in cui si desidera utilizzare tipi spaziali o se i tipi spaziali sono stati esclusi dall'installazione SQL Server, sarà necessario installarli manualmente. I tipi possono essere installati utilizzando `SQLSysClrTypes.msi`, che fa parte di Microsoft SQL Server Feature Pack. I tipi spaziali sono SQL Server specifici della versione, quindi è consigliabile [cercare "SQL Server Feature Pack"](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) nell'area download Microsoft, quindi selezionare e scaricare l'opzione che corrisponde alla versione di SQL Server che verrà usata.
