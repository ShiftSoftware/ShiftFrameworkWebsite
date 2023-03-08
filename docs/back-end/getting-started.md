# Getting Started with Shift Framework for Back-end Development

Shift Framework is powerful for developing back-end applications quickly, easily, and efficiently. What makes Shift Framework so powerful is the simplicity and easy to use approach. The framework is consist of different packages for back-end development that make it easy to build secure, scalable, and robust applications.

## Packages for Back-End Development

Each of the back-end development packages provided by Shift Framework is independent of the others. Meaning, you can only use the package you need, or all of them at once. The packages are:

- Entities: A package for working with databases and data models.
- Identity: A package for managing user authorization and authentication.
- Type Auth: A package to handle user permission throughout your application.

## Introduction

For the sake of this tutorial, we will be developing a simple ToDo API using Shift Frameworks.

### Creating a New Application

You need to create a new ASP.NET Core API and name it "ToDoAPI" (Or whatever you like to name it). You can create the project using the following command in your terminal:
``` sh
dotnet new webapi –name <Name of the project>
```
Or you can create the project using Visual Studio.