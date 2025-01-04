# Dependency Injection (DI)

## Main Concept of Dependency Injection

- The main concept behind Dependency Injection (DI) is to implement **Inversion of Control (IoC)**.
- **IoC** refers to the principle of inverting the flow of control in a system. In simpler terms, it means that objects do not create their dependencies themselves; instead, these dependencies are provided to them.
- **Loose Coupling**: By using DI, we achieve loose coupling in the code. This makes it easier to swap components and reduces the interdependence between objects.
- **Unit Testing**: DI helps in writing unit tests more easily, as dependencies can be injected via constructors or properties, allowing for easy mocking or stubbing of dependencies.

### How to Configure Dependency Injection (DI) in ASP.NET Core

- ASP.NET Core provides built-in support for DI. You do not need to manually configure a DI container or use third-party libraries.
- Dependencies are registered in the **DI container**, which in ASP.NET Core is represented by the `IServiceProvider`.
- The services are typically registered in the `ConfigureServices` method in the `Startup.cs` file of the application.

### Example of Registering Services in `ConfigureServices`

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Register services here
    services.AddTransient<IExampleService, ExampleService>();  // Example of transient service
    services.AddScoped<IAnotherService, AnotherService>();    // Example of scoped service
    services.AddSingleton<IThirdService, ThirdService>();     // Example of singleton service
}
```

### LifeTime of Services in DI

There are three main lifetimes for services in DI:

1. **Singleton**:

   - Singleton services can be registered using **AddSingleton<>** method.
   - There will be only one instance of the Singleton service throughout the application.

   Example:

   ```csharp
   services.AddSingleton<IService, Service>();
   ```

2. **Scoped**:

   - Scoped services can be registered using **AddScoped<>** method.
   - A new instance of the service is created for each request (or scope). This is useful for services that should be created once per request, like database contexts.

   Example:

   ```csharp
   services.AddScoped<IService, Service>();
   ```

3. **Transient**:

   - Transient services can be registered using **AddTransient<>** method.
   - A new instance of the service is created each time it is requested. This is useful for lightweight, stateless services.

   Example:

   ```csharp
   services.AddTransient<IService, Service>();
   ```

### Summary

- **DI** is a design pattern that promotes loose coupling and easier testing.
- ASP.NET Core's built-in DI container, `IServiceProvider`, allows you to register services with different lifetimes: Singleton, Scoped, and Transient.
- Services are typically registered in the `ConfigureServices` method within `Startup.cs`.

## TryAddSingleton, TryAddScoped and TryAddTransient methods in DI

In the context of **Dependency Injection (DI)** in .NET, the methods `TryAddSingleton`, `TryAddScoped`, and `TryAddTransient` are used to register services in the **DI container** in a way that prevents overwriting an existing registration if one already exists. This is especially useful when working with libraries or frameworks that might register services but you don't want to accidentally overwrite those registrations.

### Key Difference with Regular `AddSingleton`, `AddScoped`, and `AddTransient`

- **`AddSingleton`, `AddScoped`, and `AddTransient`**: These methods will always add a service to the DI container, even if it has already been registered. If you call them multiple times for the same service type, the last one wins, and the earlier registrations are overwritten.
- **`TryAddSingleton`, `TryAddScoped`, and `TryAddTransient`**: These methods behave similarly, but they will **only add the service if it has not already been registered**. If the service type has already been registered, the method does nothing, preventing accidental overwriting of existing registrations.

### Usage of `TryAdd` Methods

These methods are part of the **Microsoft.Extensions.DependencyInjection** namespace and are typically used in scenarios where you want to make sure a service is only registered once, even if multiple libraries or components are trying to register it.

---

### 1. `TryAddSingleton<TService, TImplementation>`

Registers a service with a **singleton** lifetime, but only if it has not already been registered. A **singleton** service is created once and shared throughout the application's lifetime.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Adds MyService as a singleton only if it's not already registered
    services.TryAddSingleton<IMyService, MyService>();
}
```

- **When to use**: Useful for ensuring a single instance of a service exists throughout the entire application. If it's already registered (perhaps by a library), it won't be overridden.

---

### 2. `TryAddScoped<TService, TImplementation>`

Registers a service with a **scoped** lifetime, but only if it has not already been registered. A **scoped** service is created once per request or operation, and each request gets its own instance.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Adds MyScopedService as scoped only if it's not already registered
    services.TryAddScoped<IMyScopedService, MyScopedService>();
}
```

- **When to use**: Useful for services that should have a separate instance per request, and you want to avoid overwriting any existing scoped service registrations.

---

### 3. `TryAddTransient<TService, TImplementation>`

Registers a service with a **transient** lifetime, but only if it has not already been registered. A **transient** service is created each time it's requested.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Adds MyTransientService as transient only if it's not already registered
    services.TryAddTransient<IMyTransientService, MyTransientService>();
}
```

- **When to use**: Useful when you want a new instance of the service every time it's requested, but you want to ensure it's not overwritten by another registration.

---

### Why Use `TryAdd` Methods?

1. **Prevent Overwriting**: If you're building an application where different modules or libraries might register services, you want to ensure that one module doesn't accidentally overwrite a service registration made by another module. The `TryAdd` methods protect against this issue.

2. **Best for Libraries**: If you're developing a library that is consumed by other projects, you can use `TryAdd` to safely add services to the DI container without risking conflicts with services already registered by the consumer's application.

3. **Improved Flexibility**: Using `TryAdd` allows for more flexible service registration in larger projects, especially when working with a mix of internal and external dependencies.

---

### Example: Scenario Where `TryAdd` Is Useful

Imagine you're developing a library and want to register a default logging service for consumers of your library, but you don't want to overwrite any logging service that might have been registered by the consuming application.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Try to add a default logger only if no other logger has been registered
    services.TryAddSingleton<ILogger, DefaultLogger>();
}
```

In this case, if the consuming application has already registered a custom `ILogger` (perhaps using `AddSingleton` or `AddScoped`), the `TryAddSingleton` method won't overwrite it. The consumer's `ILogger` will be used instead.

---

### Summary Table

| Method                      | Service Lifetime | Behavior on Existing Registration  | Use Case                                         |
| --------------------------- | ---------------- | ---------------------------------- | ------------------------------------------------ |
| `TryAddSingleton<TService>` | Singleton        | Does nothing if already registered | Use for singleton services, preventing overwrite |
| `TryAddScoped<TService>`    | Scoped           | Does nothing if already registered | Use for scoped services, preventing overwrite    |
| `TryAddTransient<TService>` | Transient        | Does nothing if already registered | Use for transient services, preventing overwrite |

---

By using `TryAdd` methods, you ensure that your services are only added if not already present, allowing for safer and more flexible service registration, especially in larger or modular applications.
