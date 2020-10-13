# Como añadir Serilog a un proyecto aspnet core

1. Instalar el paquete nuget Serilog.AspNetCore

2. Sustituir en la clase Program.cs el método Main:

````csharp

public static void Main(string[] args)
{
    Log.Logger = new LoggerConfiguration()
       .Enrich.FromLogContext()
       .WriteTo.Console()
       .WriteTo.File(new RenderedCompactJsonFormatter(), "Logs/Log.txt", rollingInterval: RollingInterval.Day)
       .WriteTo.File(new RenderedCompactJsonFormatter(), "Logs/Summary.txt", restrictedToMinimumLevel: Serilog.Events.LogEventLevel.Warning,
       rollingInterval: RollingInterval.Day)
       .CreateLogger();

    try
    {
        Log.Information("Starting up");
        CreateHostBuilder(args).Build().Run();
    }
    catch (Exception ex)
    {
        Log.Fatal(ex, "Application start-up failed");
    }
    finally
    {
        Log.CloseAndFlush();
    }
}

````

2. Configurar serilog para utilizarlo como logger en la aplicación

````
  public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseStartup<Startup>();
                }).ConfigureLogging(logging =>
                {
                    logging.ClearProviders();
                })
                 //Añadimos Serilog obteniendo la configuración desde Microsoft.Extensions.Configuration
                 .UseSerilog();
````

3. Eliminar en appsettings.json las referencias al log

4. Si se quieren logar las excepciones que ocurran (Dando por hecho que usamos el paquete Hellang.Middleware.ProblemDetails)

Añadir al agregar la dependencia de Problem details algo como esto:

````csharp

.AddProblemDetails(configure =>
{
    configure.Map<ApiException>(exception =>
    {
        Serilog.Log.Logger.Warning(exception.Message);
        return new ProblemDetails()
        {
            Title = exception.Message,
            Detail = exception.StackTrace,
            Status = StatusCodes.Status400BadRequest,
            Type = nameof(ApiException)
        };
    });
})
````

En el ejemplo anterior estamos mapeando una excepción propia (muy originalmente llamada ApiException). Cada vez que lancemos una exepción de ese tipo, el Middleware de Hellang la tratará, y lo convierte en este caso a un error 400, dando una respuesta formateada cuyo título es el nombre de la excepción.
La linea "Serilog.Log.Logger.Warning(exception.Message)" manda al log el mensaje de la excepción.

Podríamos mapear más de una excepción con este sistema.

# Utilizar un fichero de configuración

1. Al crear el logger, añadir que lea de la configuración:

````
    Log.Logger = new LoggerConfiguration()
       .ReadFrom.Configuration(Configuration)
       .CreateLogger();
````

2. Crear en la clase Program la siguiente propiedad:
````
        public static IConfiguration Configuration { get; } = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("appsettings.json", optional: false, reloadOnChange: true)
            .Build();
````

3. Agregar la configuración en el fichero appsettings.json
````
"Serilog": {
    "Using": [ "Serilog.Sinks.Console", "Serilog.Sinks.File" ],
    "WriteTo": [
      {
        "Name": "Console",
        "restrictedToMinimumLevel": "Information"
      },
      {
        "Name": "File",
        "Args": {
          "path": "Logs/Log.txt",
          "rollingInterval": 3,
          "retainedFileCountLimit": 10
        },
        "restrictedToMinimumLevel": "Warning"
      },
      {
        "Name": "File",
        "Args": {
          "path": "Logs/Summary.txt",
          "rollingInterval": 3,
          "retainedFileCountLimit": 10
        },
        "restrictedToMinimumLevel": "Warning"
      }
    ]
  }
````

4. En la llamada a UseSerilog, sustituirlo por:
````
.UseSerilog((HostBuilderContext context, LoggerConfiguration loggerConfiguration) =>
 {
     loggerConfiguration.ReadFrom.Configuration(context.Configuration);
 });
````

# Enviar al sink de Application Insights

Si la aplicación se va a subir a azure, el destino idoneo para los logs es enviarlo a Application Insights.

1. Añadir el paquete nuget *Serilog.Sinks.ApplicationInsights*

2. Añadir el sink de AI al crear el logger

````
  .WriteTo.ApplicationInsights(TelemetryConverter.Traces)
````

