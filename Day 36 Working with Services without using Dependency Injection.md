# Working with Services without Using Dependency Injection

In this tutorial, we will demonstrate how to work with services in an ASP.NET Core 5.0 Web API without utilizing Dependency Injection (DI). In typical scenarios, Dependency Injection is used to inject dependencies into controllers or services, but there may be situations where DI is not preferred, or you need a quick alternative.

We'll go through the following steps:

1. Setting up a new Web API project
2. Creating a simple service
3. Using the service in the controller without Dependency Injection
4. Accessing the service directly from the `Program.cs` file

## 1. Setting Up the Web API Project

First, create a new ASP.NET Core Web API project using the .NET CLI.

```bash
dotnet new webapi -n WithoutDIExample
cd WithoutDIExample
```

This creates a basic Web API project. You can open it in Visual Studio or any editor of your choice.

## 2. Creating a Simple Service

Let's create a simple service that we will later use in the controller. This service will be a basic class that provides a message.

1. Create a folder called `Services` and add a class named `MyService.cs` inside it.

```csharp
// Services/MyService.cs
namespace WithoutDIExample.Services
{
    public class MyService
    {
        public string GetMessage()
        {
            return "Hello from MyService without Dependency Injection!";
        }
    }
}
```

## 3. Using the Service in the Controller Without Dependency Injection

In a typical DI setup, the `MyService` class would be injected into a controller via the constructor. However, in this case, we will manually create an instance of the service in the controller.

1. Open the `Controllers` folder and modify the `WeatherForecastController.cs` file to use the `MyService` class.

```csharp
// Controllers/WeatherForecastController.cs
using Microsoft.AspNetCore.Mvc;
using WithoutDIExample.Services;

namespace WithoutDIExample.Controllers
{
    [ApiController]
    [Route("api/[controller]")]
    public class WeatherForecastController : ControllerBase
    {
        private readonly MyService _myService;

        public WeatherForecastController()
        {
            // Manually create the instance of MyService
            _myService = new MyService();
        }

        [HttpGet]
        public string Get()
        {
            // Use the service directly
            return _myService.GetMessage();
        }
    }
}
```

In this approach, we instantiate the `MyService` class manually inside the controller's constructor, bypassing Dependency Injection.

## 4. Accessing the Service Directly from `Program.cs`

Another alternative approach is to directly create and use services from the `Program.cs` file.

1. Open `Program.cs` and modify the code to manually create and use an instance of `MyService`.

```csharp
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;
using WithoutDIExample.Services;

namespace WithoutDIExample
{
    public class Program
    {
        public static void Main(string[] args)
        {
            // Create an instance of MyService without DI
            var myService = new MyService();
            var message = myService.GetMessage();
            System.Console.WriteLine(message);

            CreateHostBuilder(args).Build().Run();
        }

        public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseStartup<Startup>();
                });
    }
}
```

In this setup, we are manually creating an instance of `MyService` in the `Main` method of `Program.cs`. This is a simple and direct approach but is not a scalable solution for larger applications.

## 5. Running the Application

After setting up everything, you can now run the application using the following command:

```bash
dotnet run
```

Navigate to `http://localhost:5000/api/weatherforecast` in your browser or use Postman to send a `GET` request to the same URL. You should see the following response:

```json
"Hello from MyService without Dependency Injection!"
```

### Conclusion

In this tutorial, we've learned how to work with services in an ASP.NET Core 5.0 Web API project without relying on Dependency Injection. While Dependency Injection is the recommended and scalable approach for larger applications, there are situations where creating and using services manually might be necessary or more convenient.

Remember that avoiding DI may lead to difficulties in managing object lifetimes and could make testing harder, so it should be used with caution.
