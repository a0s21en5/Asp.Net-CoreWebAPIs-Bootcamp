# Routing

Routing in ASP.NET Core is the process of mapping an incoming HTTP request to a particular resource (usually an action method in a controller). It allows the application to handle HTTP requests dynamically and map them to appropriate methods for further processing.

### Key Concepts

1. **Resources and URLs**:

   - A **resource** refers to an object or an endpoint, usually a controller action in an ASP.NET Core Web API.
   - Every resource is accessed via a **unique URL**. However, a single resource can be associated with multiple URLs, and multiple resources cannot share the same URL.

2. **Routing Middleware**:
   - In ASP.NET Core, routing is enabled by adding two middleware components to the HTTP request pipeline:
     - `UseRouting()`: Adds routing middleware to the pipeline.
     - `UseEndpoints()`: Defines the endpoints that will handle incoming requests.

### Enabling Routing in ASP.NET Core

To enable routing in an ASP.NET Core Web application, you need to use the `UseRouting()` and `UseEndpoints()` methods in the `Configure` method of the `Startup.cs` (or `Program.cs` in .NET 6 and later).

**Example:**

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();  // Maps controller actions to routes
    });
}
```

### Setting a Route on a Controller's Action Method

In ASP.NET Core, you define routes for individual actions within controllers using attributes like `[Route]` or `[HttpGet]`, `[HttpPost]`, etc.

**Example:**

```csharp
[Route("api/[controller]")]
[ApiController]
public class ProductsController : ControllerBase
{
    [HttpGet("{id}")]
    public IActionResult GetProduct(int id)
    {
        // Logic to return a product by its ID
        return Ok(new { ProductId = id, Name = "Sample Product" });
    }
}
```

### Working with Variables in Routing

Variables in the route template allow you to pass dynamic data into controller actions. These variables are specified using `{}` in the route template.

**Example:**

```csharp
[Route("api/products/{id}")]
[HttpGet]
public IActionResult GetProduct(int id)
{
    return Ok($"Product ID: {id}");
}
```

When you send a GET request to `api/products/5`, it will invoke the `GetProduct` action with the value of `id` being `5`.

### Working with Query String in Routing

You can also capture query string parameters in ASP.NET Core routes, although they are not part of the route template itself.

**Example:**

```csharp
[Route("api/products")]
[HttpGet]
public IActionResult GetProducts([FromQuery] string category, [FromQuery] decimal? price)
{
    // Filter products based on category and price
    return Ok($"Category: {category}, Max Price: {price}");
}
```

A request to `api/products?category=electronics&price=100` would map to this action.

### Setting Multiple URLs for a Single Resource (Action Method)

You can define multiple routes for the same action method. This can be done by applying multiple `[Route]` or HTTP method attributes to the action.

**Example:**

```csharp
[Route("api/products")]
public class ProductsController : ControllerBase
{
    [HttpGet("details/{id}")]
    [HttpGet("summary/{id}")]
    public IActionResult GetProduct(int id)
    {
        return Ok($"Product ID: {id}");
    }
}
```

Both `api/products/details/{id}` and `api/products/summary/{id}` will map to the same `GetProduct` action.

### Same URL for Multiple Resources (Is This Possible?)

In ASP.NET Core, the same URL cannot map to multiple resources or controllers directly. The routing system is designed to map URLs to specific controllers and actions. If two controllers have the same route, you will encounter routing conflicts.

However, you can manage this by using different HTTP verbs (GET, POST, etc.) or adding route constraints to differentiate the actions.

### Token Replacement in Routing

You can use placeholders or tokens in route definitions, and ASP.NET Core will replace these tokens dynamically with values from the HTTP request.

**Example:**

```csharp
[Route("api/{controller}/{id}")]
[HttpGet]
public IActionResult Get(int id)
{
    return Ok($"Controller: {ControllerContext.ActionDescriptor.ControllerName}, ID: {id}");
}
```

Here, the `{controller}` token is replaced with the actual controller name (`Products` if the controller is `ProductsController`), and `{id}` is replaced with the ID passed in the URL.

### Setting the Base Route at the Controller Level

You can define a base route at the controller level that applies to all actions within that controller. The route pattern is inherited by all actions within the controller unless otherwise specified.

**Example:**

```csharp
[Route("api/products")]
public class ProductsController : ControllerBase
{
    [HttpGet]
    public IActionResult GetAllProducts()
    {
        return Ok("All products");
    }

    [HttpGet("{id}")]
    public IActionResult GetProduct(int id)
    {
        return Ok($"Product ID: {id}");
    }
}
```

In this case, the base route `api/products` is applied to all actions in the `ProductsController`, and each individual action can specify its own route further.

### Route Constraints: Validating Route Variables

Route constraints allow you to validate the values of route parameters to ensure they meet certain criteria (e.g., data type, range).

**Example:**

```csharp
[Route("api/products/{id:int}")]
[HttpGet]
public IActionResult GetProduct(int id)
{
    return Ok($"Product ID: {id}");
}
```

The `int` constraint ensures that the `id` parameter is an integer. If the route contains a non-integer value, it will result in a 404 error.

### Route Constraints Using Regular Expressions (Regex)

You can also apply regular expressions as route constraints to validate the format of route parameters.

**Example:**

```csharp
[Route("api/products/{sku:regex(^[A-Z0-9]+$)}")]
[HttpGet]
public IActionResult GetProduct(string sku)
{
    return Ok($"Product SKU: {sku}");
}
```

This route will only match if the `sku` parameter consists of uppercase letters and numbers (e.g., `SKU123`).

---

This guide covers the essential concepts and best practices for routing in ASP.NET Core, including how to define routes for controller actions, how to use route parameters, constraints, and query strings, as well as how to work with multiple routes for the same action.
