# Add default route

- **Namespace and Class**: The code is within the `ConsoleToWebAPI` namespace and contains a `Startup` class.

- **ConfigureServices Method**: Configures services used by the application. The method is currently empty, indicating no additional services beyond the defaults provided by ASP.NET Core.

- **Configure Method**: Configures the HTTP request pipeline:
  - `app.UseRouting();`: Enables routing middleware to match incoming requests to an endpoint.
  - `app.UseEndpoints()`: Configures endpoints for handling requests.
    - `endpoints.MapGet("", async context => ...)`: Maps a GET request to the root ("/") path. Responds with "Hi from New Web API App" when accessed.

## Usage

To run the application, ensure you have the necessary .NET Core SDK installed and execute the application. Access the root URL to see the response from the Web API.

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.DependencyInjection;

namespace ConsoleToWebAPI
{
    public class Startup
    {
        public void ConfigureServices(IServiceCollection services)
        {

        }
        public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
        {
            app.UseRouting();

            app.UseEndpoints(endpoints =>
            {
                endpoints.MapGet("", async context =>
                {
                    await context.Response.WriteAsync("Hi from New Web API App");
                });

                endpoints.MapGet("/Home", async context =>
                {
                    await context.Response.WriteAsync("Hi from New Web API App in Home Page");
                });
            });
        }
    }
}
```
