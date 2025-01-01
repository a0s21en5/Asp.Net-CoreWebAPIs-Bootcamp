# Creating Custom Middleware

Custom middleware in ASP.NET Core allows you to handle requests and responses as they pass through the middleware pipeline. You can use it to execute logic before the request reaches the controller or after the response is generated.

## Steps to Create Custom Middleware

1. Create the Middleware Class :
   The first step is to create a class for the middleware that will handle the request and response.

Example: `MyCustomMiddleware.cs`

```C#
using Microsoft.AspNetCore.Http;
using System;
using System.Threading.Tasks;

public class MyCustomMiddleware
{
    private readonly RequestDelegate _next;

    // Constructor accepts the next middleware in the pipeline
    public MyCustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    // Invoke method where the logic for the middleware is implemented
    public async Task InvokeAsync(HttpContext context)
    {
        // Custom logic before request is passed down the pipeline
        Console.WriteLine("Request started at: " + DateTime.Now);

        // Call the next middleware in the pipeline
        await _next(context);

        // Custom logic after response is generated
        Console.WriteLine("Response finished at: " + DateTime.Now);
    }
}
```

2. Create an Extension Method to Register the Middleware
   To make it easier to add your custom middleware to the pipeline, you can create an extension method.

Example: `MyCustomMiddlewareExtensions.cs`

```C#
public static class MyCustomMiddlewareExtensions
{
    public static IApplicationBuilder UseMyCustomMiddleware(this IApplicationBuilder builder)
    {
        return builder.UseMiddleware<MyCustomMiddleware>();
    }
}
```

3. Register the Middleware in the Pipeline

Now that the middleware and extension method are ready, you need to register your middleware in the `Startup.cs` file.

Update `Startup.cs` to Add the Middleware

1. Open Startup.cs.
2. In the Configure method, use the extension method to add your middleware.

```C#
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        // Add services to the container
        services.AddControllers();
    }

    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }
        else
        {
            app.UseExceptionHandler("/Home/Error");
            app.UseHsts();
        }

        // Add custom middleware here
        app.UseMyCustomMiddleware(); // Add this line to include custom middleware

        app.UseHttpsRedirection();
        app.UseRouting();

        app.UseAuthorization();

        app.UseEndpoints(endpoints =>
        {
            endpoints.MapControllers();
        });
    }
}
```

5. Handle Specific Routes (Optional)

You can modify the middleware to only execute for specific routes by adding conditions inside the `InvokeAsync` method.

```c#
public async Task InvokeAsync(HttpContext context)
{
    if (context.Request.Path.StartsWithSegments("/api"))
    {
        Console.WriteLine("Request started for /api endpoint: " + DateTime.Now);
    }

    await _next(context);

    if (context.Request.Path.StartsWithSegments("/api"))
    {
        Console.WriteLine("Response finished for /api endpoint: " + DateTime.Now);
    }
}
```
