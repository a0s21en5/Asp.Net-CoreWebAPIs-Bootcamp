# Dapper

In this tutorial, we will integrate **Dapper** with an **ASP.NET Core Web API** to provide fast and efficient data access. Dapper is a micro ORM (Object-Relational Mapper) for .NET that makes data access in .NET applications faster and easier, especially for simple CRUD (Create, Read, Update, Delete) operations.

## Step 1: Create a New ASP.NET Core Web API Project

1. Open Visual Studio or Visual Studio Code.
2. Create a new **ASP.NET Core Web API** project.
3. Select **API** as the project template.
4. Name the project (e.g., `DapperDemoAPI`).
5. Click **Create**.

## Step 2: Install Dapper

We need to add Dapper to the project. To do this, open the **NuGet Package Manager** or use the **Package Manager Console**.

Run the following command:

```bash
dotnet add package Dapper
```

This will install Dapper in your project.

## Step 3: Setup Database Connection

In this example, we'll be using SQL Server, but you can modify the connection string based on your database provider.

1. Open the `appsettings.json` file and add your connection string.

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=your_server;Database=your_database;User Id=your_user;Password=your_password;"
  }
}
```

2. In the `Startup.cs` file (or `Program.cs` in .NET 6+), configure the services to use the database connection.

For ASP.NET Core 3.1 or earlier (`Startup.cs`):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));
}
```

For .NET 6 and later (`Program.cs`):

```csharp
var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
builder.Services.AddControllers();
builder.Services.AddSingleton<IDbConnection>(db => new SqlConnection(builder.Configuration.GetConnectionString("DefaultConnection")));

var app = builder.Build();

app.UseAuthorization();

app.MapControllers();

app.Run();
```

## Step 4: Create a Model

Create a model that will represent the data you are working with. For example, if you're working with a `User` table, create a `User` model.

```csharp
public class User
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }
}
```

## Step 5: Create a Repository Class

Create a repository class that will handle data operations using Dapper.

```csharp
public interface IUserRepository
{
    Task<IEnumerable<User>> GetAllUsersAsync();
    Task<User> GetUserByIdAsync(int id);
    Task<int> AddUserAsync(User user);
    Task<int> UpdateUserAsync(User user);
    Task<int> DeleteUserAsync(int id);
}

public class UserRepository : IUserRepository
{
    private readonly IDbConnection _dbConnection;

    public UserRepository(IDbConnection dbConnection)
    {
        _dbConnection = dbConnection;
    }

    public async Task<IEnumerable<User>> GetAllUsersAsync()
    {
        var query = "SELECT * FROM Users";
        return await _dbConnection.QueryAsync<User>(query);
    }

    public async Task<User> GetUserByIdAsync(int id)
    {
        var query = "SELECT * FROM Users WHERE Id = @Id";
        return await _dbConnection.QueryFirstOrDefaultAsync<User>(query, new { Id = id });
    }

    public async Task<int> AddUserAsync(User user)
    {
        var query = "INSERT INTO Users (Name, Email) VALUES (@Name, @Email)";
        return await _dbConnection.ExecuteAsync(query, user);
    }

    public async Task<int> UpdateUserAsync(User user)
    {
        var query = "UPDATE Users SET Name = @Name, Email = @Email WHERE Id = @Id";
        return await _dbConnection.ExecuteAsync(query, user);
    }

    public async Task<int> DeleteUserAsync(int id)
    {
        var query = "DELETE FROM Users WHERE Id = @Id";
        return await _dbConnection.ExecuteAsync(query, new { Id = id });
    }
}
```

## Step 6: Create a Controller

Create a controller to expose the API endpoints for CRUD operations.

```csharp
[Route("api/[controller]")]
[ApiController]
public class UsersController : ControllerBase
{
    private readonly IUserRepository _userRepository;

    public UsersController(IUserRepository userRepository)
    {
        _userRepository = userRepository;
    }

    [HttpGet]
    public async Task<ActionResult<IEnumerable<User>>> GetAllUsers()
    {
        var users = await _userRepository.GetAllUsersAsync();
        return Ok(users);
    }

    [HttpGet("{id}")]
    public async Task<ActionResult<User>> GetUser(int id)
    {
        var user = await _userRepository.GetUserByIdAsync(id);
        if (user == null)
        {
            return NotFound();
        }
        return Ok(user);
    }

    [HttpPost]
    public async Task<ActionResult> AddUser([FromBody] User user)
    {
        var result = await _userRepository.AddUserAsync(user);
        return CreatedAtAction(nameof(GetUser), new { id = user.Id }, user);
    }

    [HttpPut("{id}")]
    public async Task<ActionResult> UpdateUser(int id, [FromBody] User user)
    {
        user.Id = id;
        var result = await _userRepository.UpdateUserAsync(user);
        if (result == 0)
        {
            return NotFound();
        }
        return NoContent();
    }

    [HttpDelete("{id}")]
    public async Task<ActionResult> DeleteUser(int id)
    {
        var result = await _userRepository.DeleteUserAsync(id);
        if (result == 0)
        {
            return NotFound();
        }
        return NoContent();
    }
}
```

## Step 7: Test the API

Now that everything is set up, you can run the project and test the API using tools like **Postman** or **Swagger** (if enabled).

1. Run the application (`dotnet run`).
2. Navigate to `http://localhost:5000/api/users`.
3. Use the API endpoints to test the CRUD operations:
    - `GET /api/users` – Get all users.
    - `GET /api/users/{id}` – Get a user by ID.
    - `POST /api/users` – Add a new user.
    - `PUT /api/users/{id}` – Update an existing user.
    - `DELETE /api/users/{id}` – Delete a user.

## Additional Resources

- [Dapper Documentation](https://dapper-tutorial.net/)
- [ASP.NET Core Web API Documentation](https://learn.microsoft.com/en-us/aspnet/core/web-api/?view=aspnetcore-6.0)
