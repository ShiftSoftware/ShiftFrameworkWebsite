# Getting Started with Shift Framework for Back-end Development

## Setting Up The Project

First, Open Visual Studio and create a new ``ASP.NET Core Empty`` project, and write the **Project Name** as ``ToDo.API`` and the **Solution Name** as ``ToDo``. After clicking **Next**, untick **Configure for HTTPS** option and create the project.

Second, right-click on the project solutions and select ``Add -> New Project``. Add a ``Class Library`` project and name it ``ToDo.Shared``.

Third, right-click on the project solutions again and select ``Add -> New Project``. Add a ``xUnit Test Project`` project and name it ``ToDo.Test``.

Fourth, right-click on the project solutions again and select ``Add -> New Project``. Add a ``Blazor WebAssembly App Empty`` project and name it ``ToDo.Web``. After clicking **Next**, untick **Configure for HTTPS** option and create the project.

### Adding The Project References

First, Go to **ToDo.API** and right-click on **Dependencies**, then select ``Add Project Reference..``. Then tick **ToDo.Shared** and **ToDo.Web** projects and click **okay**.

Second, Go to **ToDo.Test** and right-click on **Dependencies**, then select ``Add Project Reference..``. Then tick **ToDo.API** project and click **Okay**.

Third, Go to **ToDo.Web** and right-click on **Dependencies**, then select ``Add Project Reference..``. Then tick **ToDo.Shared** project and click **Okay**.

## Adding The Needed Folders

Right-click on **ToDo.API** project and select ``Add -> New Folder``, then add the following folders with the same structure.

```
Controllers
Data
    -> Entities
    -> Repositories
```

After that, right-click on **ToDo.Shared** project and select `` Add -> New Folder``, then add the following folders.

```
DTOs
Enums
```

Lastly, right-click on **ToDo.Web** project and select `` Add -> New Folder``, then add the following folders.

```
Services
Shared
```