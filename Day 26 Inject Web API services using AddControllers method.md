# Setting Up ASP.NET Core Web API Application

## ConfigureServices Method

The `ConfigureServices` method is responsible for configuring services that the application will use. In the provided example, `services.AddControllers()` is used to add support for controllers that handle incoming HTTP requests. This method is where you configure dependency injection, middleware, and other services required by the application.

## Configure Method

The `Configure` method is where the HTTP request processing pipeline is configured. Here's a breakdown of the steps in the example:

1. **UseRouting**: This middleware enables routing within the application, allowing it to match incoming requests to the appropriate endpoint handlers based on routes defined in controllers.

2. **UseEndpoints**: This method is used to define the endpoints for handling requests. In the example, `endpoints.MapControllers()` is used to map HTTP requests to actions in the controllers.

### Example Code

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.DependencyInjection;

namespace ConsoleToWebAPI
{
    public class Startup
    {
        // ConfigureServices method to configure services
        public void ConfigureServices(IServiceCollection services)
        {
            // Add controllers to handle HTTP requests
            services.AddControllers();
        }

        // Configure method to set up the HTTP request pipeline
        public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
        {
            // Enable routing middleware
            app.UseRouting();

            // Define endpoint mappings for controllers
            app.UseEndpoints(endpoints =>
            {
                // Map controllers to handle HTTP requests
                endpoints.MapControllers();
            });
        }
    }
}
