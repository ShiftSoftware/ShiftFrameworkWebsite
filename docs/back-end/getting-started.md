# Getting Started with Shift Framework for Back-end Development

## Setting Up The Project

Open Visual Studio and create a new ``ASP.NET Core Empty`` project, and write the **Project Name** as ``ToDo.API`` and the **Solution Name** as ``ToDo``. After clicking **Next**, untick **Configure for HTTPS** option (Untick this option for all the project you create throughout this tutorial) and create the project.

Then, create the following projects inside the same solution by right-clicking on the project solution and selecting ``Add -> New Project``:

1. ``Class Library``, name it ``ToDo.Shared``.
2. ``xUnit Test Project``, name it ``ToDo.Test``.
3. ``Blazor WebAssembly App Empty``, name it ``ToDo.Web``.

??? info

    If you want to know how to create the rest of the projects, please follow the following steps:

    1. right-click on the project solutions and select ``Add -> New Project``. Add a ``Class Library`` project and name it ``ToDo.Shared``.

    2. right-click on the project solutions again and select ``Add -> New Project``. Add a ``xUnit Test Project`` project and name it ``ToDo.Test``.

    3. right-click on the project solutions again and select ``Add -> New Project``. Add a ``Blazor WebAssembly App Empty`` project and name it ``ToDo.Web``. After clicking **Next**, untick **Configure for HTTPS** option and create the project.

The project structure should look like this:

```
.
├── ToDo.API
│
├── ToDo.Shared
│
├── ToDo.Test
│
├── ToDo.Web
```

### Adding The Project References

Go to **ToDo.API** and right-click on **Dependencies**, then select ``Add Project Reference..``. Then tick **ToDo.Shared** and **ToDo.Web** projects and click **OK**.

- [x] ToDo.Shared
- [ ] ToDo.Test
- [x] ToDo.Web

For **ToDo.Test**, tick **ToDo.API**.

- [x] ToDo.API
- [ ] ToDo.Shared
- [ ] ToDo.Web

For **ToDo.Web**, tick **ToDo.Shared**.

- [ ] ToDo.API
- [x] ToDo.Shared
- [ ] ToDo.Test

## Adding The Needed Folders

Right-click on **ToDo.API** project and select ``Add -> New Folder``, then add the following folders:

``` hl_lines="6-9"
.
├── ToDo.API
│   ├── Connected Services
│   ├── Dependencies
│   ├── Properties
│   ├── Controllers
│   ├── Data
│   │   ├── Entities
│   │   ├── Repositories
│   ├── appsettings.json
│   ├── Program.cs
│
├── ToDo.Shared
│
├── ToDo.Test
│
├── ToDo.Web
```

Inside **ToDo.Shared** add the following folders:

``` hl_lines="6-7"
.
├── ToDo.API
│
├── ToDo.Shared
│   ├── Dependencies
│   ├── DTOs
│   ├── Enums
│
├── ToDo.Test
│
├── ToDo.Web
```

Inside **ToDo.Web** add the following folders:

``` hl_lines="14-15"
.
├── ToDo.API
│
├── ToDo.Shared
│
├── ToDo.Test
│
├── ToDo.Web
│   ├── Connected Services
│   ├── Dependencies
│   ├── Properties
│   ├── wwwroot
│   ├── Pages
│   ├── Services
│   ├── Shared
│   ├── _Imports.razor
│   ├── App.razor
│   ├── MainLayout.razor
│   ├── Program.cs
```