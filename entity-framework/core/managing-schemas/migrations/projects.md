---
title: Migrazioni con progetti multipli - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 76e88dd486b1c53dc69a24e35710511bf9cb673b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997987"
---
<a name="using-a-separate-project"></a>Usando un progetto separato
========================
È possibile archiviare le migrazioni in un assembly diverso da quello contenente il `DbContext`. È anche possibile usare questa strategia per gestire più set di migrazioni, ad esempio, uno per lo sviluppo e altro per gli aggiornamenti di versione di rilascio.

Per eseguire questa operazione...

1. Creare una nuova libreria di classi.

2. Aggiungere un riferimento all'assembly di DbContext.

3. Spostare le migrazioni e i file di snapshot del modello alla libreria di classi.
   > [!TIP]
   > Se non si dispone di alcuna migrazione esistente, generarne uno del progetto contenente l'oggetto DbContext, spostarlo. Questo è importante perché se l'assembly di migrazioni non contiene una migrazione esistente, il comando Add-Migration sarà Impossibile trovare l'oggetto DbContext.

4. Configurare l'assembly delle migrazioni:

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. Aggiungere un riferimento all'assembly le migrazioni da assembly di avvio.
   * Se in questo modo una dipendenza circolare, aggiornare il percorso di output della libreria di classi:

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

Se è stato eseguito tutto correttamente, sarà possibile aggiungere le nuove migrazioni al progetto.

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
