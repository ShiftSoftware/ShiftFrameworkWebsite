# Introduction to Type Auth Packages

Type Auth packages can help in the implementation of authentication and authorization features in.NET applications. There are three packages available in Type Auth, each with a specific focus: 

- `TypeAuth.Core`: This package provides a core set of functionality for handling user permissions throughout your application. It's a great starting point for any project that needs to manage user roles and permissions.

- `TypeAuth.AspNetCore`: If you're building an ASP .NET Core application, this package can help you handle authentication and authorization in a way that integrates seamlessly with the framework. It includes middleware for validating user credentials and enforcing access control.

- `TypeAuth.Blazor`: If you're using Blazor to build your UI, this package can help you manage user view permissions. It provides components that can be used to conditionally render parts of the UI based on a user's role or permissions.

All three packages are available on NuGet.org and can be installed via the NuGet Package Manager in Visual Studio or using the `dotnet` command line tool.

``` sh
dotnet add package ShiftSoftware.<Write the package name that you need here>
```
