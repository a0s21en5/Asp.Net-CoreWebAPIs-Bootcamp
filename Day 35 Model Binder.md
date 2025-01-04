# Model Binder

Model binding is the process of binding HTTP request data to the parameters of application controllers or properties. It is an essential concept in web development frameworks like ASP.NET Core. There are built-in methods and attributes for model binding, and developers can also create custom model binders to meet specific needs.

## Built-In Model Binding Methods and Attributes

ASP.NET Core provides several built-in attributes for model binding. Some of the most commonly used attributes include:

### 1. **BindProperty Attribute**

- **Purpose**: The `BindProperty` attribute is used to map incoming form data to public properties in a controller or model.
- **Usage**: It is applied to each target property individually.
- **Works with**: Both simple data types (e.g., `int`, `string`) and complex data objects.
- **Limitations**: By default, the `BindProperty` does not work with HTTP GET requests.
- **Controller Level**: It can be applied at the controller level for individual properties.

### Example

```csharp
public class MyController : Controller
{
    [BindProperty]
    public string Name { get; set; }

    public IActionResult OnPost()
    {
        // 'Name' will automatically be populated with the form data
        return View();
    }
}
```

### 2. **Default Way of Data Binding**

- **Simple Types**: Simple types like `int`, `string`, `float`, etc., are bound to URL data (query parameters or route data).
- **Complex Types**: Complex types (e.g., classes, models) are bound from the body of the HTTP request.

### 3. **FromQuery Attribute**

- **Purpose**: The `FromQuery` attribute is used to bind data available in the query string.
- **Example**: If you send a request like `http://localhost:8700/api/countries?name=india&area=30`, the `name` and `area` parameters will be automatically bound to the corresponding parameters or properties in the controller.

```csharp
[HttpGet]
public IActionResult GetCountry([FromQuery] string name, [FromQuery] int area)
{
    // 'name' and 'area' will be populated from the query string
    return Ok(new { name, area });
}
```

### 4. **FromRoute Attribute**

- **Purpose**: The `FromRoute` attribute is used to bind data from the URL route.
- **Example**: For a route like `http://localhost:8700/api/employee/2/departments/3`, the `2` and `3` values would be automatically bound to the corresponding route parameters.

```csharp
[HttpGet("api/employee/{employeeId}/departments/{departmentId}")]
public IActionResult GetDepartment(int employeeId, int departmentId)
{
    // 'employeeId' and 'departmentId' will be populated from the route
    return Ok(new { employeeId, departmentId });
}
```

### 5. **FromBody Attribute**

- **Purpose**: The `FromBody` attribute is used to bind the request body to a parameter or property, typically for complex objects (like JSON).
- **Example**:

```csharp
[HttpPost]
public IActionResult CreateEmployee([FromBody] Employee employee)
{
    // 'employee' will be populated from the body of the POST request
    return Ok(employee);
}
```

### 6. **FromForm Attribute**

- **Purpose**: The `FromForm` attribute is used to bind data from form data (e.g., from a `<form>` HTML element).
- **Example**:

```csharp
[HttpPost]
public IActionResult SubmitForm([FromForm] string name, [FromForm] int age)
{
    // 'name' and 'age' will be populated from the form data in the HTTP request
    return Ok(new { name, age });
}
```

### 7. **FromHeader Attribute**

- **Purpose**: The `FromHeader` attribute is used to bind data from the HTTP request header.
- **Example**:

```csharp
[HttpGet]
public IActionResult GetUser([FromHeader(Name = "X-User-Id")] string userId)
{
    // 'userId' will be populated from the custom header 'X-User-Id'
    return Ok(new { userId });
}
```

## Custom Model Binder

While ASP.NET Core provides many built-in model binding features, you might need to implement a **custom model binder** for specific use cases. A custom model binder allows you to control how model binding occurs based on the request data.

### Steps to Create a Custom Model Binder

1. **Implement IModelBinder Interface**: You need to create a class that implements the `IModelBinder` interface, which defines the binding logic.

2. **Create a Model Binder Provider**: To register your custom model binder, you may need to create a custom model binder provider.

3. **Use the Custom Binder**: You can use your custom model binder by applying the `ModelBinder` attribute to your model properties.

### Example of a Custom Model Binder

```csharp
public class CustomDateBinder : IModelBinder
{
    public Task BindModelAsync(ModelBindingContext bindingContext)
    {
        var value = bindingContext.ValueProvider.GetValue("date").FirstValue;
        if (DateTime.TryParse(value, out var date))
        {
            bindingContext.Result = ModelBindingResult.Success(date);
        }
        else
        {
            bindingContext.ModelState.AddModelError("date", "Invalid date format.");
            bindingContext.Result = ModelBindingResult.Failed();
        }
        return Task.CompletedTask;
    }
}

public class MyModel
{
    [ModelBinder(BinderType = typeof(CustomDateBinder))]
    public DateTime MyDate { get; set; }
}
```

### Registering the Custom Binder

In `Startup.cs` or `Program.cs` (depending on your version of ASP.NET Core), register the custom model binder:

```csharp
services.AddControllers(options =>
{
    options.ModelBinderProviders.Insert(0, new CustomModelBinderProvider());
});
```
