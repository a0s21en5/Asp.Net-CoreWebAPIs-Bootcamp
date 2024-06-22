# Setting up ASP.NET Core Application with `ConfigureWebHostDefaults`

- Provides support for HTTP

- Sets the kestrel server as the web server.

- Enables the IIS Integration.

- Etc

The provided code snippet demonstrates the setup of an ASP.NET Core application using the `ConfigureWebHostDefaults` method. Here's a breakdown of what each part does:

- **Main Method (`Program.cs`):**
  - This is the entry point of the application (`Main` method).
  - It calls `CreateHostBuilder(args)` to build and run the application.

- **CreateHostBuilder Method (`Program.cs`):**
  - This method configures the host for the ASP.NET Core application.
  - It uses `Host.CreateDefaultBuilder(args)` to set up a default host configuration.
  - `ConfigureWebHostDefaults` is then called on the `IHostBuilder` to configure the web host.
  - Inside `ConfigureWebHostDefaults`, `UseStartup<Startup>()` specifies the `Startup` class that configures the application.

- **Startup Class (`Startup.cs`):**
  - The `Startup` class is where you configure the application's services and middleware.
  - It typically includes methods like `ConfigureServices` and `Configure` for configuring dependency injection and request handling pipeline, respectively.

This setup effectively initializes an ASP.NET Core application with Kestrel as the web server, integrates with IIS (if hosting on Windows), and allows configuration through the `Startup` class.

For further customization, additional services, middleware, or configuration can be added within the `Startup` class depending on the requirements of the application.

## In Program class add some code in CreateHostBuilder method in below main method and create one Startup class for this

```c#
class Program
{
    static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args).ConfigureWebHostDefaults(webHost =>
        {
            webHost.UseStartup<Startup>();
        });
}
```
