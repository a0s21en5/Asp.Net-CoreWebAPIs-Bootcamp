# Automapper

Automapper is a powerful object-to-object mapper that helps streamline mapping between objects in .NET applications. It eliminates the need for manual mapping code and simplifies data transformation between layers in an ASP.NET Core Web API project.

## Why Use Automapper?

In a Web API, you often need to map domain models to DTOs (Data Transfer Objects) and vice versa. Writing mapping logic manually can lead to repetitive code and maintenance challenges. Automapper simplifies this by automating the mapping process.

---

## Getting Started

### Step 1: Install Automapper

Install the `AutoMapper.Extensions.Microsoft.DependencyInjection` NuGet package.

```bash
dotnet add package AutoMapper.Extensions.Microsoft.DependencyInjection
```

---

### Step 2: Configure Automapper

1. **Create Mapping Profiles**  
   Define mapping rules by creating a class that inherits from `Profile`.

   ```csharp
   using AutoMapper;

   public class MappingProfile : Profile
   {
       public MappingProfile()
       {
           CreateMap<SourceModel, DestinationModel>();
           CreateMap<DestinationModel, SourceModel>();
       }
   }

   public class SourceModel
   {
       public int Id { get; set; }
       public string Name { get; set; }
   }

   public class DestinationModel
   {
       public int Id { get; set; }
       public string Name { get; set; }
   }
   ```

2. **Register Automapper in `Startup.cs`**

   ```csharp
   public void ConfigureServices(IServiceCollection services)
   {
       services.AddAutoMapper(typeof(Startup)); // Registers all profiles in the assembly
       services.AddControllers();
   }
   ```

---

### Step 3: Use Automapper in Controllers

Inject `IMapper` into your controller and use it to map objects.

```csharp
using AutoMapper;
using Microsoft.AspNetCore.Mvc;

[ApiController]
[Route("api/[controller]")]
public class ExampleController : ControllerBase
{
    private readonly IMapper _mapper;

    public ExampleController(IMapper mapper)
    {
        _mapper = mapper;
    }

    [HttpGet]
    public IActionResult Get()
    {
        var source = new SourceModel { Id = 1, Name = "Source" };
        var destination = _mapper.Map<DestinationModel>(source);

        return Ok(destination);
    }
}
```

---

## Advanced Usage

### 1. Custom Value Resolvers

You can customize how specific properties are mapped.

```csharp
public class CustomResolver : IValueResolver<SourceModel, DestinationModel, string>
{
    public string Resolve(SourceModel source, DestinationModel destination, string destMember, ResolutionContext context)
    {
        return source.Name.ToUpper();
    }
}

public class MappingProfile : Profile
{
    public MappingProfile()
    {
        CreateMap<SourceModel, DestinationModel>()
            .ForMember(dest => dest.Name, opt => opt.MapFrom<CustomResolver>());
    }
}
```

### 2. Ignore Properties

Exclude properties from mapping.

```csharp
CreateMap<SourceModel, DestinationModel>()
    .ForMember(dest => dest.Name, opt => opt.Ignore());
```

### 3. Reverse Mapping

Use `ReverseMap()` to enable two-way mapping.

```csharp
CreateMap<SourceModel, DestinationModel>().ReverseMap();
```

---

## Testing Automapper

Automapper mappings can be tested using unit tests.

```csharp
using AutoMapper;
using Xunit;

public class MappingTests
{
    private readonly IMapper _mapper;

    public MappingTests()
    {
        var config = new MapperConfiguration(cfg =>
        {
            cfg.AddProfile<MappingProfile>();
        });
        _mapper = config.CreateMapper();
    }

    [Fact]
    public void Should_Map_Source_To_Destination()
    {
        var source = new SourceModel { Id = 1, Name = "Test" };
        var destination = _mapper.Map<DestinationModel>(source);

        Assert.Equal(source.Id, destination.Id);
        Assert.Equal(source.Name, destination.Name);
    }
}
```
