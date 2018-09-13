---
title: Configurazione basata su codice - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
ms.openlocfilehash: 7c7da8992fd1b29f998e08c13d445c1d2d8cc2ce
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490818"
---
# <a name="code-based-configuration"></a>Configurazione basata su codice
> [!NOTE]
> **Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6. Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.  

Configurazione per un'applicazione Entity Framework può essere specificata in un file di configurazione (app.config/web.config) o tramite codice. Quest'ultimo è nota come configurazione basata su codice.  

Configurazione in un file di configurazione è descritta una [articolo separato](config-file.md). Il file di configurazione ha la precedenza sulla configurazione basata su codice. In altre parole, se è impostata un'opzione di configurazione in entrambe le code e nel file di configurazione, viene utilizzata l'impostazione nel file di configurazione.  

## <a name="using-dbconfiguration"></a>Usando DbConfiguration  

Configurazione basata su codice in EF6 e versioni successive è possibile creare una sottoclasse di System.Data.Entity.Config.DbConfiguration. Le seguenti linee guida da seguire quando si crea una sottoclasse DbConfiguration:  

- Creare una sola classe DbConfiguration per l'applicazione. Questa classe specifica le impostazioni a livello di dominio applicazione.  
- Inserire la classe DbConfiguration nello stesso assembly come la classe DbContext. (Vedere la *lo spostamento DbConfiguration* sezione se si desidera modificare questa impostazione.)  
- Assegnato alla classe DbConfiguration un costruttore pubblico senza parametri.  
- Impostare le opzioni di configurazione tramite la chiamata di metodi DbConfiguration protetti all'interno di questo costruttore.  

Attenendosi alle linee guida consente a EF di individuare e usare la configurazione automaticamente da entrambi gli strumenti che necessita di accedere al modello e quando viene eseguita l'applicazione.  

## <a name="example"></a>Esempio  

Una classe derivata da DbConfiguration potrebbe essere simile al seguente:  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

namespace MyNamespace
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            SetDefaultConnectionFactory(new LocalDBConnectionFactory("mssqllocaldb"));
        }
    }
}
```  

Questa classe imposta per usare la strategia di esecuzione di SQL Azure - per ripetere automaticamente le operazioni di database non riuscita - e Usa Local DB per i database creati per convenzione da Code First di Entity Framework.  

## <a name="moving-dbconfiguration"></a>Lo spostamento DbConfiguration  

Esistono casi in cui non è possibile inserire la classe DbConfiguration nello stesso assembly come la classe DbContext. Ad esempio, potrebbe essere due DbContext ciascuna delle classi in assembly diversi. Sono disponibili due opzioni per gestirla.  

La prima opzione è usare il file di configurazione per specificare l'istanza DbConfiguration da utilizzare. A tale scopo, impostare l'attributo codeConfigurationType della sezione entityFramework. Ad esempio:  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

Il valore di codeConfigurationType deve essere l'assembly e il nome completo dello spazio dei nomi della classe DbConfiguration.  

La seconda opzione consiste nell'inserire DbConfigurationTypeAttribute sulla classe del contesto. Ad esempio:  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

Il valore passato all'attributo può essere di tipo DbConfiguration: come illustrato in precedenza - o stringa del nome tipo completo di assembly e dello spazio dei nomi. Ad esempio:  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a>Impostare in modo esplicito DbConfiguration  

Esistono alcune situazioni in cui configurazione potrebbe essere necessari prima di qualsiasi tipo DbContext è stato utilizzato. Alcuni esempi sono:  

- Uso di DbModelBuilder per compilare un modello senza un contesto  
- Uso di altri framework/codice di alcune utilità che usa un oggetto DbContext in cui tale contesto viene usato prima che venga utilizzato il contesto dell'applicazione  

In tali situazioni Entity Framework è in grado di individuare automaticamente la configurazione ed è invece necessario eseguire una delle operazioni seguenti:  

- Impostare il tipo DbConfiguration nel file di configurazione, come descritto nel *lo spostamento DbConfiguration* sezione precedente
- Chiamare il metodo DbConfiguration.SetConfiguration statico durante l'avvio dell'applicazione  

## <a name="overriding-dbconfiguration"></a>Si esegue l'override DbConfiguration  

Esistono alcune situazioni in cui è necessario eseguire l'override di impostazioni di configurazione nella DbConfiguration. Ciò non avviene in genere dagli sviluppatori di applicazioni, ma piuttosto da provider di terze parti e plug-in non è possibile usare una classe derivata DbConfiguration.  

A tale scopo, Entity Framework consente a un gestore eventi da registrare che consentono di modificare la configurazione esistente subito prima che venga bloccato.  Fornisce inoltre un metodo sugar in modo specifico per la sostituzione di qualsiasi servizio restituito dal localizzatore di servizi di Entity Framework. Si tratta di come si dovrà essere utilizzato:  

- All'avvio dell'app (prima che venga utilizzato Entity Framework) il plug-in o provider deve registrare il metodo del gestore eventi per questo evento. (Si noti che ciò deve verificarsi prima che l'applicazione usa Entity Framework).  
- Il gestore dell'evento effettua una chiamata a ReplaceService per ogni servizio che deve essere sostituito.  

Repalce IDbConnectionFactory e DbProviderService, ad esempio, si potrebbe registrare un gestore simile al seguente:  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

Nel codice sopra MyProviderServices e MyConnectionFactory rappresentano le implementazioni del servizio.  

È anche possibile aggiungere i gestori delle dipendenze aggiuntive per ottenere lo stesso effetto.  

Si noti che è anche possibile eseguire il wrapping DbProviderFactory in questo modo, ma tale operazione così influiranno solo Entity Framework e non utilizzi di DbProviderFactory all'esterno di EF. Per questo motivo è opportuno continuare a eseguire il wrapping DbProviderFactory come prima.  

È inoltre necessario tenere presenti i servizi che si esegue all'esterno dell'applicazione, ad esempio, durante l'esecuzione di migrazioni dalla Console di gestione pacchetti. Quando si esegue la migrazione dalla console di che tenterà di trovare i DbConfiguration. Tuttavia, se otterrà il servizio sottoposto a wrapping dipende da dove il gestore dell'evento è stato registrato. Se è registrato come parte della costruzione del DbConfiguration deve eseguire il codice e il servizio è necessario ottenere il wrapping. In genere non si tratterà il case e ciò significa che gli strumenti non otterranno il servizio sottoposto a wrapping.  
