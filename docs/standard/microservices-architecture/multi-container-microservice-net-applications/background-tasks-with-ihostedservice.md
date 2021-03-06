---
title: Implementieren von Hintergrundtasks in Microservices mit IHostedService und der BackgroundService-Klasse
description: .NET-Microservicesarchitektur für .NET-Containeranwendungen | Implementieren von Hintergrundtasks in Microservices mit IHostedService und der BackgroundService-Klasse
author: CESARDELATORRE
ms.author: wiwagn
ms.date: 12/11/2017
ms.openlocfilehash: 981a20ca80f0652a9c3597d36b960d6b44d97912
ms.sourcegitcommit: c93fd5139f9efcf6db514e3474301738a6d1d649
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2018
ms.locfileid: "50195826"
---
# <a name="implement-background-tasks-in-microservices-with-ihostedservice-and-the-backgroundservice-class"></a>Implementieren von Hintergrundtasks in Microservices mit IHostedService und der BackgroundService-Klasse

Ab einem bestimmten Zeitpunkt müssen möglicherweise in auf Microservices basierende Anwendungen oder auch in anderen Anwendungen Hintergrundtasks und geplante Aufträge implementiert werden. Die Besonderheit bei der Verwendung einer Microservicesarchitektur ist, dass Sie einen einzelnen Microserviceprozess oder -container implementieren können, um diese Hintergrundtasks zu hosten. Dadurch kann bei Bedarf eine vertikale Skalierung vorgenommen und zusätzlich sichergestellt werden, dass nur eine Instanz des Microserviceprozesses oder -containers ausgeführt wird.

In .NET Core werden diese Tasks als gehostete Dienste bezeichnet, da sie Dienste oder Logiken darstellen, die in einem Host, in einer Anwendung oder in einem Microservice gehostet werden. Beachten Sie, dass in diesem Fall der gehostete Dienst einfach einer Klasse mit der Logik des Hintergrundtasks entspricht.

Seit .NET Core 2.0 stellt das Framework die neue Schnittstelle <xref:Microsoft.Extensions.Hosting.IHostedService> bereit, mit der Sie gehostete Dienste einfach implementieren können. Die Grundidee besteht darin, mehrere Hintergrundtasks (gehostete Dienste) zu registrieren, die neben dem Host oder Webhost im Hintergrund ausgeführt werden. Dies wird in der folgenden Abbildung dargestellt.

![](./media/image26.png)

**Abbildung 8-25.** Verwenden von IHostedService mit Host und WebHost

Beachten Sie den Unterschied zwischen `WebHost` und `Host`. `WebHost` (eine Basisklasse, die `IWebHost` implementiert) ist in ASP.NET Core 2.0 das Infrastrukturartefakt, mit dem Sie HTTP-Serverfunktionen für Ihren Prozess (z.B. das Implementieren einer MVC-Webanwendung oder eines Web-API-Diensts) bereitstellen. Diese Klasse enthält in ASP.NET Core alle optimierten Infrastrukturfunktionen, mit der Sie Dependency Injection verwenden, der HTTP-Pipeline Middlewareanwendungen hinzufügen und `IHostedServices` für Hintergrundtasks nutzen können.

`Host` (eine Basisklasse, die `IHost` implementiert) stellt jedoch in .NET Core 2.1 eine Neuerung dar. Mit `Host` können Sie eine ähnliche Infrastruktur wie mit `WebHost` (Dependency Injection, gehostete Dienste usw.) verwenden. Der Unterschied besteht darin, dass Sie einen einfacheren und schlankeren Prozess für den Host verwenden und auf Verknüpfungen mit MVC, einer Web-API oder HTTP-Serverfunktionen verzichten.

Sie können daher entweder einen spezialisierten Hostprozess mit IHost erstellen, um ausschließlich gehostete Dienste (beispielsweise einen Microservice zum Hosten von `IHostedServices`) zu verarbeiten, oder aber einen vorhandenen ASP.NET Core-`WebHost` wie eine ASP.NET Core-Web-API oder eine MVC-Anwendung erweitern. 

Je nach Geschäfts- und Skalierbarkeitsanforderungen hat jeder Ansatz seine Vor- und Nachteile. Falls Ihre Hintergrundtasks nichts mit HTTP (IWebHost) zu tun haben und Sie .NET Core 2.1 verwenden, wird empfohlen, IHost zu verwenden.

## <a name="registering-hosted-services-in-your-webhost-or-host"></a>Registrieren von gehosteten Diensten in WebHost oder Host

Im Folgenden wird die `IHostedService`-Schnittstelle genauer beschrieben, da sie in `WebHost` und `Host` ähnlich verwendet wird. 

SignalR ist ein Beispiel für ein Artefakt, das gehostete Dienste verwendet. Sie können den Dienst aber auch für einfachere Aufgaben verwenden. Hierzu zählt etwa Folgendes:

-   ein Hintergrundtask, der eine Datenbank abruft und nach Änderungen sucht
-   ein geplanter Task, der einen Cache regelmäßig aktualisiert
-   eine Implementierung von QueueBackgroundWorkItem, durch die ein Task in einem Hintergrundthread ausgeführt werden kann
-   das Verarbeiten von Nachrichten aus einer Nachrichtenwarteschlange im Hintergrund einer Webanwendung, während allgemeine Dienste wie `ILogger` gemeinsam genutzt werden.
-   ein Hintergrundtask, der mit `Task.Run()` gestartet wird

Alle Aktionen lassen sich in einen Hintergrundtask auslagern, der auf IHostedService basiert.

Sie können einem `WebHost` oder `Host` ein oder mehrere `IHostedServices` hinzufügen, indem Sie sie über den standardmäßigen Dependency Injection-Vorgang in einem ASP.NET Core-`WebHost` (oder in einem `Host` in .NET Core 2.1) registrieren. Dabei müssen Sie die gehosteten Dienste in der bekannten `ConfigureServices()`-Methode der `Startup`-Klasse registrieren, was im folgenden Codeausschnitt einer typischen ASP.NET-WebHost-Klasse demonstriert wird. 

```csharp
public IServiceProvider ConfigureServices(IServiceCollection services)
{            
    //Other DI registrations;

    // Register Hosted Services
    services.AddSingleton<IHostedService, GracePeriodManagerService>();
    services.AddSingleton<IHostedService, MyHostedServiceB>();
    services.AddSingleton<IHostedService, MyHostedServiceC>();
    //...
}
```

Der Code für den gehosteten Dienst `GracePeriodManagerService` entspricht dem Code aus dem Microservice für Bestellungen in eShopOnContainers. Bei den anderen beiden Diensten handelt es sich hingegen nur um zwei zusätzliche Beispiele.

Die Ausführung des `IHostedService`-Hintergrundtasks wird mit der Lebensdauer der Anwendung (also des Hosts oder des Microservices) koordiniert. Die Tasks werden registriert, wenn die Anwendung gestartet und eine ordnungsgemäße Aktion ausgeführt oder wenn die Anwendung beendet wird und dabei Ressourcen bereinigt werden.

Ohne die Nutzung von `IHostedService` ließe sich ein Hintergrundthread erstellen, in dem ein beliebiger Task ausgeführt werden könnte. Zum Beendigungszeitpunkt der Anwendung würde der Thread dann einfach beendet werden, und ordnungsgemäße Aktionen zur Bereinigung von Ressourcen könnten nicht ausgeführt werden.


## <a name="the-ihostedservice-interface"></a>Die IHostedService-Schnittstelle

Beim Registrieren von `IHostedService` ruft .NET Core die Methoden `StartAsync()` und `StopAsync()` des Typs `IHostedService` während des Anwendungsstarts und -stopps auf. Nach dem Serverstart wird die Startmethode aufgerufen, und `IApplicationLifetime.ApplicationStarted` wird ausgelöst.

Die `IHostedService`-Schnittstelle sieht in .NET Core wie folgt aus:

```csharp
namespace Microsoft.Extensions.Hosting
{
    //
    // Summary:
    //     Defines methods for objects that are managed by the host.
    public interface IHostedService
    {
        //
        // Summary:
        // Triggered when the application host is ready to start the service.
        Task StartAsync(CancellationToken cancellationToken);
        //
        // Summary:
        // Triggered when the application host is performing a graceful shutdown.
        Task StopAsync(CancellationToken cancellationToken);
    }
}
```
Sie können mehrere Implementierungen von IHostedService erstellen und wie zuvor gezeigt in der `ConfigureService()`-Methode des Dependency Injection-Containers registrieren. Alle gehosteten Dienste werden zusammen mit der Anwendung und dem Microservice gestartet und beendet.

Als Entwickler sind Sie dafür verantwortlich, die Beendigungsaktion Ihrer Dienste zu verarbeiten, wenn die `StopAsync()`-Methode vom Host ausgelöst wird.

## <a name="implementing-ihostedservice-with-a-custom-hosted-service-class-deriving-from-the-backgroundservice-base-class"></a>Implementieren von IHostedService mit einer eigenen Klasse für gehostete Dienste, die von der Basisklasse BackgroundService abgeleitet wird

Sie haben die Möglichkeit, eine eigene Klasse für gehostete Dienste neu zu erstellen und `IHostedService` zu implementieren, was im Fall von .NET Core 2.0 sogar unumgänglich ist. 

Da allerdings die meisten Hintergrundtasks ähnliche Anforderungen an die Verwaltung von Abbruchtoken und an weitere typische Vorgänge stellen, bietet .NET Core 2.1 hierfür die praktische abstrakte Basisklasse BackgroundService an, von der eigene Klassen abgeleitet werden können.

Diese Klasse fasst die wesentlichen Aufgaben zusammen, die zum Einrichten der Hintergrundtasks erforderlich sind. Beachten Sie, dass diese Klasse bereits in der Bibliothek für .NET Core 2.1 enthalten ist. Sie brauchen sie daher nicht selbst zu schreiben.

Noch wurde .NET Core 2.1 jedoch nicht freigegeben. Aus diesem Grund wird in der eShopOnContainers-Anwendung, für die aktuell .NET Core 2.0 verwendet wird, nur vorübergehend diese Klasse aus dem Open Source-Repository für .NET Core 2.1 eingebunden (nur die Open Source-Lizenz ist erforderlich, eine proprietäre Lizenz wird nicht benötigt), da sie mit der aktuellen IHostedService-Schnittstelle in .NET Core 2.0 kompatibel ist. Sobald .NET Core 2.1 freigegeben wird, müssen Sie lediglich auf das richtige NuGet-Paket verweisen.

Im folgenden Codeausschnitt ist die in .NET Core 2.1 implementierte abstrakte Basisklasse BackgroundService zu sehen.

```csharp
// Copyright (c) .NET Foundation. Licensed under the Apache License, Version 2.0. 
/// <summary>
/// Base class for implementing a long running <see cref="IHostedService"/>.
/// </summary>
public abstract class BackgroundService : IHostedService, IDisposable
{
    private Task _executingTask;
    private readonly CancellationTokenSource _stoppingCts = 
                                                   new CancellationTokenSource();

    protected abstract Task ExecuteAsync(CancellationToken stoppingToken);

    public virtual Task StartAsync(CancellationToken cancellationToken)
    {
        // Store the task we're executing
        _executingTask = ExecuteAsync(_stoppingCts.Token);

        // If the task is completed then return it, 
        // this will bubble cancellation and failure to the caller
        if (_executingTask.IsCompleted)
        {
            return _executingTask;
        }

        // Otherwise it's running
        return Task.CompletedTask;
    }
    
    public virtual async Task StopAsync(CancellationToken cancellationToken)
    {
        // Stop called without start
        if (_executingTask == null)
        {
            return;
        }

        try
        {
            // Signal cancellation to the executing method
            _stoppingCts.Cancel();
        }
        finally
        {
            // Wait until the task completes or the stop token triggers
            await Task.WhenAny(_executingTask, Task.Delay(Timeout.Infinite,
                                                          cancellationToken));
        }

    }

    public virtual void Dispose()
    {
        _stoppingCts.Cancel();
    }
}
```

Wenn Sie eine Klasse aus der oben gezeigten abstrakten Basisklasse ableiten, müssen Sie dank der vererbten Implementierung nur die `ExecuteAsync()`-Methode in Ihrer eigenen Klasse für gehostete Dienste implementieren. Dies wird im folgenden vereinfachten Codeausschnitt aus eShopOnContainers demonstriert, in dem eine Datenbank abgerufen und Integrationsereignisse bei Bedarf im Ereignisbus veröffentlicht werden.

```csharp
public class GracePeriodManagerService : BackgroundService
{        
    private readonly ILogger<GracePeriodManagerService> _logger;
    private readonly OrderingBackgroundSettings _settings;

    private readonly IEventBus _eventBus;

    public GracePeriodManagerService(IOptions<OrderingBackgroundSettings> settings,
                                     IEventBus eventBus,
                                     ILogger<GracePeriodManagerService> logger)
        {
            //Constructor’s parameters validations...       
        }

        protected override async Task ExecuteAsync(CancellationToken stoppingToken)
        {
            _logger.LogDebug($"GracePeriodManagerService is starting.");

            stoppingToken.Register(() => 
                    _logger.LogDebug($" GracePeriod background task is stopping."));

            while (!stoppingToken.IsCancellationRequested)
            {
                _logger.LogDebug($"GracePeriod task doing background work.");

                // This eShopOnContainers method is querying a database table 
                // and publishing events into the Event Bus (RabbitMS / ServiceBus)
                CheckConfirmedGracePeriodOrders();

                await Task.Delay(_settings.CheckUpdateTime, stoppingToken);
            }
            
            _logger.LogDebug($"GracePeriod background task is stopping.");

        }

        protected override async Task StopAsync (CancellationToken stoppingToken)
        {
               // Run your graceful clean-up actions
        }
}
```

Im Fall von eShopOnContainers wird eine Anwendungsmethode ausgeführt, durch die eine Datenbanktabelle abgefragt wird. Diese sucht nach Bestellungen mit einem bestimmten Zustand. Beim Übernehmen von Änderungen werden Integrationsereignisse über den Ereignisbus veröffentlicht (im Hintergrund kann RabbitMQ oder Azure Service Bus verwendet werden). 

Sie könnten allerdings auch andere Geschäftshintergrundtasks ausführen.

Standardmäßig wird für das Abbruchtoken ein Zeitlimit von 5 Sekunden festgelegt, obwohl der Wert beim Erstellen von `WebHost` mithilfe der `UseShutdownTimeout`-Erweiterung von `IWebHostBuilder` geändert werden kann. Der Dienst muss also innerhalb von fünf Sekunden beendet werden, da er ansonsten sofort beendet wird.

Im Folgenden Codeausschnitt wird dieses Zeitlimit geändert und auf 10 Sekunden festgelegt.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

### <a name="summary-class-diagram"></a>Zusammenfassendes Klassendiagramm

In Abbildung 8-26 werden zusammenfassend die Klassen und Schnittstellen dargestellt, die an der Implementierung von IHostedService beteiligt sind.
 
![](./media/image27.png)

**Abbildung 8-26.** Klassendiagramm mit mehreren Klassen und Schnittstellen, die mit IHostedService in Verbindung stehen

### <a name="deployment-considerations-and-takeaways"></a>Überlegungen zur Bereitstellung und wesentliche Erkenntnisse

Sie sollten beachten, dass die konkrete Bereitstellung von ASP.NET Core-`WebHost` oder .NET Core-`Host` Ihre finale Lösung beeinflussen kann. Wenn Sie `WebHost` beispielsweise auf IIS oder in der regulären Azure App Service-Lösung bereitstellen, kann der Host durch den Neustart eines Anwendungspools heruntergefahren werden. Wenn Sie den Host jedoch als Container in einem Orchestrator wie Kubernetes oder Service Fabric bereitstellen, können Sie die zugesicherte Anzahl der aktiven Hostinstanzen anpassen. Darüber hinaus ist es empfehlenswert, sich mit anderen cloudbasierten Lösungen wie Azure Functions zu befassen, die speziell für derartige Szenarios entworfen wurden. Wenn Sie möchten, dass der Dienst permanent ausgeführt und auf einer Windows Server-Instanz bereitgestellt wird, verwenden Sie einen Windows-Dienst.

Auch wenn `WebHost` in einem Anwendungspool bereitgestellt würde, müssten andere Szenarios wie das Leeren oder erneute Auffüllen des speicherinternen Anwendungscaches berücksichtigt werden.

Die `IHostedService`-Schnittstelle bietet eine einfache Möglichkeit, Hintergrundtasks in einer ASP.NET Core-Webanwendung (.NET 2.0 Core) oder in einem Prozess oder Host zu starten (ab .NET Core 2.1 mit `IHost`). Der Hauptvorteil besteht darin, dass Sie durch einen ordnungsgemäßen Abbruch Bereinigungscode in Ihren Hintergrundtasks ausführen können, wenn der Host heruntergefahren wird.


#### <a name="additional-resources"></a>Zusätzliche Ressourcen

-   **Building a scheduled task in ASP.NET Core/Standard 2.0 (Erstellen eines geplanten Tasks in ASP.NET Core/Standard 2.0)** 

    [*https://blog.maartenballiauw.be/post/2017/08/01/building-a-scheduled-cache-updater-in-aspnet-core-2.html*](https://blog.maartenballiauw.be/post/2017/08/01/building-a-scheduled-cache-updater-in-aspnet-core-2.html)

-   **Implementing IHostedService in ASP.NET Core 2.0 (Implementieren von IHostedService in ASP.NET Core 2.0)** 

    [*https://www.stevejgordon.co.uk/asp-net-core-2-ihostedservice*](https://www.stevejgordon.co.uk/asp-net-core-2-ihostedservice)

-   **ASP.NET Core 2.1 Hosting samples (Hostingbeispiele für ASP.NET Core 2.1)** 

    [*https://github.com/aspnet/Hosting/tree/release/2.1/samples/GenericHostSample*](https://github.com/aspnet/Hosting/tree/release/2.1/samples/GenericHostSample)

>[!div class="step-by-step"]
[Zurück](test-aspnet-core-services-web-apps.md)
[Weiter](../microservice-ddd-cqrs-patterns/index.md)
