# Controller Class for Web API

To implement a controller class in a Web API application, follow these guidelines:

- **Naming Convention**: Name your controller class with a "Controller" suffix, such as `SampleController`.

- **Inheritance**: Ensure your controller class inherits from `ControllerBase`.

- **ApiController Attribute**: Apply the `[ApiController]` attribute to the controller class. This attribute enables several features including attribute routing, automatic HTTP 400 responses for client errors, inferred support for multipart/form-data requests, and data binding from incoming request data to action method parameters.

- **Attribute Routing**: Utilize attribute routing to define endpoints directly on your action methods within the controller.

## ControllerBase

The `ControllerBase` class provides essential methods and properties to handle HTTP requests within the ASP.NET Core Web API framework.

## ApiController Attribute

The `[ApiController]` attribute in ASP.NET Core Web API:

- Enables attribute routing for defining endpoints directly on action methods.
- Automatically handles HTTP 400 errors (Bad Request) when model validation fails.
- Supports inferred handling of multipart/form-data requests.
- Facilitates automatic binding of incoming request data to action method parameters through additional attributes.

## # ASP.NET Core Application Configuration

## Middleware: Developer Exception Page

The `UseDeveloperExceptionPage` middleware in ASP.NET Core provides detailed error information during development.

### Description

The `UseDeveloperExceptionPage` middleware intercepts unhandled exceptions in HTTP requests and displays a developer-friendly exception page. This page includes:

- Stack traces
- Headers
- Query strings
- Form data
- Other diagnostic information

This is invaluable for debugging during development. However, it should only be enabled in development environments (`env.IsDevelopment()`) to prevent exposing sensitive information in production.

### Configuration Example

In the `Configure` method of the `Startup` class:

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
}
```
