---
title: Usando migrate.exe - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: 989ea862-e936-4c85-926a-8cfbef5df5b8
ms.openlocfilehash: 39740578e4a8c2d5400bcabbcb107baf0648fba5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993499"
---
# <a name="using-migrateexe"></a>Usando migrate.exe
Migrazioni Code First consente di aggiornare un database da visual Studio, ma possono essere eseguite anche tramite il migrate.exe strumento della riga di comando. Questa pagina fornirà una panoramica rapida su come usare migrate.exe per eseguire le migrazioni in un database.

> [!NOTE]
> Questo articolo presuppone che l'utente sappia usare migrazioni Code First in scenari di base. Se in caso contrario, allora sarà necessario leggere [migrazioni Code First](~/ef6/modeling/code-first/migrations/index.md) prima di continuare.

## <a name="copy-migrateexe"></a>Copiare migrate.exe

Quando si installa Entity Framework con NuGet migrate.exe sarà all'interno della cartella degli strumenti del pacchetto scaricato. Nelle &lt;cartella del progetto&gt;\\pacchetti\\EntityFramework.&lt; versione&gt;\\strumenti

Dopo aver migrate.exe quindi è necessario copiarlo nel percorso dell'assembly che contiene le migrazioni.

Se l'applicazione è destinata a .NET 4 e 4.5 non, quindi sarà necessario copiare il **Redirect.config** in una posizione e e rinominarlo **migrate.exe.config**. Si tratta in modo che migrate.exe Ottiene i reindirizzamenti di associazione corretto per essere in grado di individuare l'assembly di Entity Framework.

| .NET 4.5                                   | .NET 4.0                                   |
|:-------------------------------------------|:-------------------------------------------|
| ![Net45Files](~/ef6/media/net45files.png)  | ![Net40Files](~/ef6/media/net40files.png)  |

> [!NOTE]
> migrate.exe nepodporuje funkci x64 degli assembly.

## <a name="using-migrateexe"></a>Usando Migrate.exe

Dopo aver spostato migrate.exe nella cartella corretta, quindi sarà possibile usarlo per eseguire migrazioni rispetto ai database. L'utilità è progettato per eseguire operazioni solo eseguire le migrazioni. Non può generare le migrazioni o creare uno script SQL.

### <a name="see-options"></a>Vedere le opzioni

``` console
Migrate.exe /?
```

Il codice precedente visualizzerà la pagina della Guida associata a questa utilità, tenere presente che dovrai avere il file EntityFramework. dll nello stesso percorso che si eseguono migrate.exe in ordine per il corretto funzionamento.

### <a name="migrate-to-the-latest-migration"></a>Eseguire la migrazione per la migrazione di più recente

``` console
Migrate.exe MyMvcApplication.dll /startupConfigurationFile=”..\\web.config”
```

Quando l'esecuzione di migrate.exe il parametro solo obbligatorio è l'assembly, ovvero l'assembly che contiene le migrazioni che si siano tentando di eseguire, ma userà convenzione di tutte le impostazioni in base al se non si specifica il file di configurazione.

### <a name="migrate-to-a-specific-migration"></a>Eseguire la migrazione a una migrazione specifica

``` console
Migrate.exe MyApp.exe /startupConfigurationFile=”MyApp.exe.config” /targetMigration=”AddTitle”
```

Se si desidera eseguire le migrazioni da un massimo di una migrazione specifica, è possibile specificare il nome della migrazione. Tutte le migrazioni precedente verrà eseguito in base alle esigenze fino alla Guida alla migrazione specificata.

### <a name="specify-working-directory"></a>Specificare directory di lavoro

``` console
Migrate.exe MyApp.exe /startupConfigurationFile=”MyApp.exe.config” /startupDirectory=”c:\\MyApp”
```

Se si assembly ha dipendenze o legge i file rispetto alla directory di lavoro è necessario impostare startupDirectory.

### <a name="specify-migration-configuration-to-use"></a>Specificare la configurazione della migrazione da usare

``` console
Migrate.exe MyAssembly CustomConfig /startupConfigurationFile=”..\\web.config”
```

Se si dispone di più classi di configurazione della migrazione, le classi che ereditano da DbMigrationConfiguration, quindi è necessario specificare che deve essere utilizzato per l'esecuzione. Questo è specificato, fornendo il secondo parametro opzionale senza un commutatore come sopra.

### <a name="provide-connection-string"></a>Specificare la stringa di connessione

``` console
Migrate.exe BlogDemo.dll /connectionString=”Data Source=localhost;Initial Catalog=BlogDemo;Integrated Security=SSPI” /connectionProviderName=”System.Data.SqlClient”
```

Se si vuole specificare una stringa di connessione nella riga di comando, è necessario fornire anche il nome del provider. Non specificare il nome del provider genera un'eccezione.

## <a name="common-problems"></a>Problemi comuni

| Messaggio di errore                                                                                                                                                                                                                                                                                                                      | Soluzione                                                                                                                                                                                                                                                                                             |
|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Eccezione non gestita: System.IO.FileLoadException: Impossibile caricare il file o l'assembly ' EntityFramework, versione = versione=5.0.0.0, lingua = neutral, PublicKeyToken = b77a5c561934e089' o una delle relative dipendenze. Definizione del manifesto dell'assembly individuato non corrisponde il riferimento all'assembly. (Eccezione da HRESULT: 0x80131040)         | In genere, ciò significa che si esegue un'applicazione .NET 4 senza file Redirect.config. È necessario copiare il Redirect.config nello stesso percorso migrate.exe e rinominarlo in migrate.exe.config.                                                                                       |
| Eccezione non gestita: System.IO.FileLoadException: Impossibile caricare il file o l'assembly ' EntityFramework, versione = 4.4.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089' o una delle relative dipendenze. Definizione del manifesto dell'assembly individuato non corrisponde il riferimento all'assembly. (Eccezione da HRESULT: 0x80131040)          | Questa eccezione indica che si sta eseguendo un'applicazione con il Redirect.config copiato nel percorso migrate.exe di .NET 4.5. Se l'app è .NET 4.5 non è necessario avere il file di configurazione tramite il reindirizzamento all'interno. Eliminare il file migrate.exe.config.                                    |
| Errore: Non è possibile aggiornare database in modo che corrisponda al modello corrente perché sono presenti modifiche in sospeso e la migrazione automatica è disabilitata. Scrivere le modifiche al modello in sospeso per una migrazione basata su codice o abilitare la migrazione automatica. Impostare DbMigrationsConfiguration.AutomaticMigrationsEnabled a true per abilitare la migrazione automatica. | Questo errore si verifica se la migrazione in esecuzione quando non è stata creata una migrazione per far fronte alle modifiche apportate al modello e il database corrisponde al modello. Aggiunta di una proprietà a una classe di modello esegue quindi migrate.exe senza la creazione di una migrazione per aggiornare il database è un esempio di questo oggetto. |
| Errore: Tipo non risolto per il membro ' System.Data.Entity.Migrations.Design.ToolingFacade+UpdateRunner,EntityFramework, versione = versione=5.0.0.0, lingua = neutral, PublicKeyToken = b77a5c561934e089'.                                                                                                                                       | Questo errore può essere causato specificando una directory di avvio non validi. Deve essere il percorso del migrate.exe                                                                                                                                                                                      |
| Eccezione non gestita: System. NullReferenceException: riferimento oggetto non impostato su un'istanza di un oggetto. <br/>   in System.Data.Entity.Migrations.Console.Program.Main (String [] args)                                                                                                                                             | Ciò può essere causato, non specificare un parametro obbligatorio per uno scenario che si sta utilizzando. Ad esempio si specifica una stringa di connessione senza specificare il nome del provider.                                                                                                                        |
| Errore: più di un tipo di configurazione di migrazioni è stato trovato nell'assembly 'ClassLibrary1'. Specificare il nome di quello da usare.                                                                                                                                                                                                  | L'errore, come indicato è presente più di una classe di configurazione nell'assembly specificato. È necessario utilizzare l'opzione /configurationType per specificare quale usare.                                                                                                                                           |
| Errore: Impossibile caricare il file o l'assembly '&lt;assemblyName&gt;' o una delle relative dipendenze. L'assembly specificato assegnare un nome o alla codebase non è valido. (Eccezione da HRESULT: 0x80131047)                                                                                                                                                    | Ciò può essere causato specificando un nome di assembly in modo non corretto o non avere                                                                                                                                                                                                                          |
| Errore: Impossibile caricare il file o l'assembly '&lt;assemblyName&gt;' o una delle relative dipendenze. Tentativo di caricare un programma con un formato non corretto.                                                                                                                                                                          | Ciò si verifica se si sta tentando di eseguire migrate.exe x64 dell'applicazione. Entity Framework 5.0 e versioni precedenti sono applicabili solo ai x86.                                                                                                                                                                                |
