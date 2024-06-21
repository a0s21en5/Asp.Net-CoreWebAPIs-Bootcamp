# Convert a Console Application to Web API

## Program.cs

- Add the Web Host Builder

- Configure the Startup Class.

In Program class add new method (CreateHostBuilder) in below main method

```C#
public static IHostBuilder CreateHostBuilder()
{
    Host.CreateDefaultBuilder();
}
```

## Startup

- Add Routing

- Set the default route

## What is Host Builder

- Host builder is an object and it is used to add some features in the application.

## CreateDefaultBuilder

- Dependency Injection(DI)

- Configuration

  - appSetting.json
  - appSetting.{env}.json
  - User secrets
  - Environment Variables
  - Etc

- Logging

  - Console, Debug, etc.
  - Set the content root path to the result of GetCurrentDirectory() method.
