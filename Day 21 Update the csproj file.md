# Convert a Console Application to Web API Project

## CSPROJ

- Change the project SDK to `Microsoft.NET.Sdk.Web`.
- Remove the `<OutputType>` element.
- Verify the target framework moniker (TFM).

## Console Application (Original)

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net5.0</TargetFramework>
  </PropertyGroup>

</Project>
```

## Converted Web API Project

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>net5.0</TargetFramework>
  </PropertyGroup>

</Project>
```
