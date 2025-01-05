# Setting Up the `DbContext` Class and Database Connection String

This guide walks you through the steps required to configure the `DbContext` class and database connection string in an ASP.NET Core 5.0 Web API project.

---

## 1. **Install Required NuGet Packages**

Ensure you have the necessary packages installed for Entity Framework Core. Run the following commands in the **Package Manager Console** or use the **NuGet Package Manager**:

```bash
Install-Package Microsoft.EntityFrameworkCore
Install-Package Microsoft.EntityFrameworkCore.SqlServer
Install-Package Microsoft.EntityFrameworkCore.Tools
```

---

## 2. **Create the `AppDbContext` Class**

Add a new class file named `AppDbContext.cs` to the `Data` folder (or create a new folder if it doesn't exist).

```csharp
using Microsoft.EntityFrameworkCore;

namespace YourNamespace.Data
{
    public class AppDbContext : DbContext
    {
        public AppDbContext(DbContextOptions<AppDbContext> options) : base(options) { }

        // Define your DbSets here
        public DbSet<Product> Products { get; set; }
        public DbSet<Cart> Carts { get; set; }
        // Add more DbSets as needed
    }
}
```

Replace `YourNamespace` with your project's namespace, and define your entity classes like `Product` and `Cart`.

---

## 3. **Configure the Connection String**

Open the `appsettings.json` file and add your database connection string. Replace `<YourDatabaseName>` with the name of your database.

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\MSSQLLocalDB;Database=<YourDatabaseName>;Trusted_Connection=True;"
  }
}
```

---

## 4. **Register `DbContext` in `Startup.cs`**

In the `ConfigureServices` method of the `Startup.cs` file, register the `AppDbContext` with the dependency injection container.

```csharp
using Microsoft.EntityFrameworkCore;
using YourNamespace.Data;

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddDbContext<AppDbContext>(options =>
            options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

        services.AddControllers();
        // Add other services
    }
}
```

---

## 5. **Run Migrations and Update the Database**

Use the following commands to create and apply migrations to set up your database schema:

1. **Add Migration**  
   Run the following command to generate a migration script:

   ```bash
   dotnet ef migrations add InitialCreate
   ```

2. **Update Database**  
   Apply the migration to your database:

   ```bash
   dotnet ef database update
   ```

---

## 6. **Test the Configuration**

- Verify that the database has been created in your SQL Server instance.
- Use a controller or service to perform CRUD operations and validate the setup.

---

## Example Entity Class: `Product`

```csharp
namespace YourNamespace.Models
{
    public class Product
    {
        public int ProductID { get; set; }
        public string ProductName { get; set; }
        public decimal Price { get; set; }
    }
}
```
