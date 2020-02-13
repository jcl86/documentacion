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

2. Añadir .UseSerilog() despues de CreateDefaultBuilder(args)

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
