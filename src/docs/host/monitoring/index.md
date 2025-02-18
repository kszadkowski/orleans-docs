---
title: Runtime Monitoring
---

# Orleans Logs

Orleans leverages [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/) for all silo and client logs.

# Runtime Monitoring

Orleans outputs its runtime statistics and metrics through the `ITelemetryConsumer` interface.
Application can register one or more telemetry consumers with for their silos and clients, to receives statistics and metrics that Orleans runtime  periotically publishes.
These can be consumers for popular telemetry analytics solutions or custom ones for any other destination and purpose.
Three telemetry consumer are currently included in the Orleans codebase.

They are released as separate NuGet packages: 

- `Microsoft.Orleans.OrleansTelemetryConsumers.AI` for publishing to [Application Insights](https://azure.microsoft.com/en-us/services/application-insights/).

- `Microsoft.Orleans.OrleansTelemetryConsumers.Counters` for publishing to Windows performance counters.
The Orleans runtime continually updates a number of them.
CounterControl.exe tool, included in the [`Microsoft.Orleans.CounterControl`](https://www.nuget.org/packages/Microsoft.Orleans.CounterControl/) NuGet package, helps register necessary performance counter categories.
It has to run with elevated privileges.
The performance counters can be monitored using any of the standard monitoring tools.

- `Microsoft.Orleans.OrleansTelemetryConsumers.NewRelic`, for publishing to [New Relic](https://newrelic.com/).

To configure your silo and client to use telemetry consumers, silo configuration code looks like this: 
```c#
var siloHostBuilder = new SiloHostBuilder();
//configure the silo with AITelemetryConsumer
siloHostBuilder.AddApplicationInsightsTelemetryConsumer("INSTRUMENTATION_KEY");
```

Client configuration code look like this: 
```c#
var clientBuilder = new ClientBuilder();
//configure the clientBuilder with AITelemetryConsumer
clientBuilder.AddApplicationInsightsTelemetryConsumer("INSTRUMENTATION_KEY");
```

To use a custom defined TelemetryConfiguration (which may have TelemetryProcessors, TelemetrySinks, etc.), silo configuration code looks like this: 
```c#
var siloHostBuilder = new SiloHostBuilder();
var telemetryConfiguration = TelemetryConfiguration.CreateDefault();
//configure the silo with AITelemetryConsumer
siloHostBuilder.AddApplicationInsightsTelemetryConsumer(telemetryConfiguration);
```

Client configuration code look like this: 
```c#
var clientBuilder = new ClientBuilder();
var telemetryConfiguration = TelemetryConfiguration.CreateDefault();
//configure the clientBuilder with AITelemetryConsumer
clientBuilder.AddApplicationInsightsTelemetryConsumer(telemetryConfiguration);
```