# Create first Web API project

- Using CLI
- Using Visual Studio

# Creating an ASP.NET Core 5.0 Web API Project Using CLI

Follow these steps to create an ASP.NET Core 5.0 Web API project using the .NET CLI (Command Line Interface).

## 1. Open Command Prompt (Windows) or Terminal (Mac/Linux)

Open your preferred command-line interface.

## 2. Navigate to Your Project Directory

Use the `cd` command to navigate to the directory where you want to create your project.

## 3. Create a New ASP.NET Core Web API Project

Run the following command to create a new ASP.NET Core Web API project:

```bash
dotnet new webapi -n MyWebApi
```

This command creates a new ASP.NET Core Web API project named "MyWebApi".

## 4. Navigate to the Project Directory

Use the `cd` command to navigate into the newly created project directory:

```bash
cd MyWebApi
```

## 5. Restore Dependencies

Run the following command to restore the dependencies required by your project:

```bash
dotnet restore
```

## 6. Run the Application

You can now run your ASP.NET Core Web API application using the following command:

```bash
dotnet run
```

This command will build and run your application. Once it's running, you should see output indicating that the application has started.

## 7. Test the API

Open a web browser or a tool like Postman and navigate to the URL `https://localhost:5001/api/values`. This is the default endpoint for a new Web API project. You should see some sample JSON data returned, indicating that your API is working.
