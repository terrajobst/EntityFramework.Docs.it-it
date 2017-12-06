---
title: "Migrazioni di più progetti - Core a Entity Framework"
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: e059deb6c7555a4f6732fe7942855e95b3f9a34c
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
---
<a name="using-a-separate-project"></a>Utilizzo di un progetto separato
========================
Si consiglia di archiviare le migrazioni in un assembly diverso da quello contenente il `DbContext`. È anche possibile utilizzare questa strategia per gestire più set di migrazioni, ad esempio, una per lo sviluppo e un'altra per gli aggiornamenti di versione di rilascio.

Per eseguire questa operazione...

1. Creare una nuova libreria di classi.

2. Aggiungere un riferimento all'assembly DbContext.

3. Spostare i file di snapshot di modello e le migrazioni alla libreria di classi.
   * Se non sono stati aggiunti, aggiungerne uno al progetto DbContext quindi spostarlo.

4. Configurare l'assembly di migrazioni:

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. Aggiungere un riferimento all'assembly le migrazioni da assembly di avvio.
   * Se in questo modo una dipendenza circolare, aggiornare il percorso di output della libreria di classi:

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStarupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

Se sono state eseguite tutte le operazioni in modo corretto, sarà possibile aggiungere le migrazioni di nuovo al progetto.

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migraitons add NewMigration --project MyApp.Migrations
```
