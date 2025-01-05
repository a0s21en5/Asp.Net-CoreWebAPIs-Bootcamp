# Read connection string from appsettings.json file

In an ASP.NET Core 5.0 Web API application, the connection string for the database is typically stored in the `appsettings.json` file. Here's a step-by-step guide on how to read the connection string:

---

## **1. Add the Connection String to `appsettings.json`**

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=YOUR_SERVER_NAME;Database=YOUR_DATABASE_NAME;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

## **2. Configure the `Startup.cs` File**

### Import Required Namespaces

Add the necessary namespaces at the top of your `Startup.cs` file:

```csharp
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.EntityFrameworkCore;
```

### Configure Services

In the `ConfigureServices` method, configure the DbContext with the connection string:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Reading the connection string from appsettings.json
    var connectionString = Configuration.GetConnectionString("DefaultConnection");

    // Add DbContext with SQL Server
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(connectionString));

    // Other services
    services.AddControllers();
}
```

## **3. Create the DbContext Class**

Create a new class (e.g., `ApplicationDbContext`) inheriting from `DbContext`:

```csharp
using Microsoft.EntityFrameworkCore;

public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    // Define your DbSet properties for your models
    public DbSet<Product> Products { get; set; }
}
```

## **4. Inject the DbContext into Controllers**

Inject the `ApplicationDbContext` into your API controllers where needed:

```csharp
using Microsoft.AspNetCore.Mvc;

[ApiController]
[Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    private readonly ApplicationDbContext _context;

    public ProductsController(ApplicationDbContext context)
    {
        _context = context;
    }

    [HttpGet]
    public async Task<IActionResult> GetProducts()
    {
        var products = await _context.Products.ToListAsync();
        return Ok(products);
    }
}
```

## **5. Run the Application**

- Ensure the database exists or is created (if using migrations).
- Run the application and test the API.

---

### **Notes**

1. **Sensitive Data**: Avoid hardcoding sensitive data in `appsettings.json`. For production, use secure methods like environment variables or Azure Key Vault.
2. **Connection String Syntax**: Adjust the connection string syntax for the type of database you're using (e.g., SQL Server, MySQL, PostgreSQL).

Let me know if you need help with a specific step!
