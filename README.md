# Dependency_Injection-PrimitiveCalculator-Task2

This is the solution for "Task #2: initialize the calculator using DI-container (from Microsoft, for example)"

```csharp
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using System;

namespace Calculator
{
    class Program
    {
        static void Main(string[] args)
        {
            var host = CreateHostBuilder(args).Build();

            using (var serviceScope = host.Services.CreateScope())
            {
                var serviceProvider = serviceScope.ServiceProvider;
                var calculator = serviceProvider.GetRequiredService<PrimitiveCalculator>();

                ConsoleKey exitKey = ConsoleKey.A;
                while (exitKey != ConsoleKey.D0 && exitKey != ConsoleKey.NumPad0)
                {
                    calculator.Run();

                    Console.WriteLine("To exit press 0: ");
                    exitKey = Console.ReadKey().Key;
                }
            }
        }

        static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .ConfigureServices((hostContext, services) =>
                {
                    services.Configure<DisplayOptions>(hostContext.Configuration.GetSection("DisplayOptions"));

                    services.AddSingleton<IMenu, Menu>();
                    services.AddSingleton<IOperationRunner, OperationRunner>();
                    services.AddTransient<ICalculatingOperation, Add>();
                    services.AddTransient<ICalculatingOperation, Subtract>();
                    services.AddTransient<ICalculatingOperation, Multiply>();
                    services.AddTransient<ICalculatingOperation, Divide>();
                    services.AddSingleton<PrimitiveCalculator>();
                });
    }
}
```



