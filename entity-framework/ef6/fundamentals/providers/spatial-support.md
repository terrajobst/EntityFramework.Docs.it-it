---
title: Supporto del provider per i tipi spaziali - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: 9c00e82c663daec219fe649a8d889afcc81564f7
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022275"
---
# <a name="provider-support-for-spatial-types"></a>Supporto del provider per i tipi spaziali
Entity Framework supporta l'utilizzo di dati spaziali tramite le classi di DbGeography o DbGeometry. Queste classi si basano su funzionalità specifiche del database offerte dal provider di Entity Framework. Non tutti i provider supportano i dati spaziali e quelli che si possono avere i prerequisiti aggiuntivi, ad esempio l'installazione degli assembly di tipo spaziale. Altre informazioni sul supporto del provider per i tipi spaziali sono disponibili sotto.  

Sono disponibili informazioni aggiuntive su come usare i tipi spaziali in un'applicazione in due procedure dettagliate, uno per Code First, l'altra per i Database First o Model First:  

- [Tipi di dati spaziali nel codice prima di tutto](~/ef6/modeling/code-first/data-types/spatial.md)  
- [Tipi di dati spaziali nella finestra di progettazione di Entity Framework](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a>Versioni di Entity Framework che supportano i tipi spaziali  

Supporto per i tipi spaziali è stato introdotto in EF5. Tuttavia, in EF5 tipi spaziali sono supportati solo quando l'applicazione ha come destinazione e viene eseguita in .NET 4.5.  

A partire da Entity Framework 6 tipi di dati spaziali sono supportati per le applicazioni destinate a .NET 4 sia .NET 4.5.  

## <a name="ef-providers-that-support-spatial-types"></a>Provider di Entity Framework che supportano i tipi spaziali  

### <a name="ef5"></a>EF5  

I provider di Entity Framework per EF5 che siamo a conoscenza di che supportano i tipi spaziali sono:  

- Provider di Microsoft SQL Server  
    - Questo provider viene fornito come parte di EF5.  
    - Questo provider dipende da alcune librerie aggiuntive di basso livello che potrebbero dover essere installata, ovvero per i dettagli vedere di seguito.  
- [Devart dotConnect per Oracle](http://www.devart.com/dotconnect/oracle/)  
    - Si tratta di un provider di terze parti da Devart.  

Se si conosce di provider che supporta i tipi spaziali, quindi EF5 contattarci e saremo lieti di aggiungerlo a questo elenco.  

### <a name="ef6"></a>EF6  

I provider di Entity Framework per Entity Framework 6 che siamo a conoscenza di che supportano i tipi spaziali sono:  

- Provider di Microsoft SQL Server  
    - Questo provider viene fornito come parte di Entity Framework 6.  
    - Questo provider dipende da alcune librerie aggiuntive di basso livello che potrebbero dover essere installata, ovvero per i dettagli vedere di seguito.  
- [Devart dotConnect per Oracle](http://www.devart.com/dotconnect/oracle/)  
    - Si tratta di un provider di terze parti da Devart.  

Se si sa di un provider di Entity Framework 6 che supporta i tipi spaziali quindi, contatto e di Microsoft sarà lieta di aggiungerlo a questo elenco.  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a>Prerequisiti per i tipi spaziali con Microsoft SQL Server  

Supporto spaziale di SQL Server dipende dai tipi specifici di SQL Server a basso livello, SqlGeometry e SqlGeography. In tempo reale di questi tipi in assembly Microsoft.SqlServer.Types.dll, e questo assembly non viene fornito come parte di Entity Framework o come parte di .NET Framework.  

Quando è installato Visual Studio viene installato spesso anche una versione di SQL Server e installazione del Microsoft.SqlServer.Types.dll includerà.  

Se SQL Server non è installato nel computer in cui si desidera utilizzare i tipi spaziali o tipi di dati spaziali sono stati esclusi dall'installazione di SQL Server, sarà necessario installarli manualmente. I tipi possono essere installati usando `SQLSysClrTypes.msi`, che fa parte del Feature Pack di Microsoft SQL Server. Tipi di dati spaziali sono SQL Server specifiche della versione, pertanto si consiglia [cercare "SQL Server Feature Pack"](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) nel Microsoft Download Center, quindi selezionare e scaricare l'opzione che corrisponde alla versione di SQL Server che verrà utilizzato.
