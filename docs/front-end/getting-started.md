# Getting Started with Shift Framework for Front-end Development

## Installation

!!! Note

    This tutorial is a continuation of the back-end. In order to follow this tutorial, please implement the back-end first.

Inside the ToDoAPI project in Visual Studio, right click on the project solution and select ``New -> Project``. Then, create a new ``Blazor WebAssembly App`` project and name it ``ToDoUI``.

!!! Note

    If you create the project inside Visual Studio, please make sure to untick ``Configure for HTTPS`` option.

Then, you need to install ``ShiftSoftware.ShiftBlazor`` package. Either, using Visual Studio Package Manager, or .NET CLI command line.

``` sh
dotnet add package ShiftSoftware.ShiftBlazor --version 1.0.12-alpha
```

## Setting Up the Project

First, go to ``Program.cs`` and add the Shift Services to the services.

``` cs hl_lines="6"
var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient { BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) });
builder.Services.AddShiftServices(config => { });

await builder.Build().RunAsync();
```

After that, go to ``_imports.razor`` file and add the following dependencies.

``` razor hl_lines="8-11"
@using System.Net.Http
@using System.Net.Http.Json
@using Microsoft.AspNetCore.Components.Routing
@using Microsoft.AspNetCore.Components.Web
@using Microsoft.AspNetCore.Components.WebAssembly.Http
@using Microsoft.JSInterop
@using ToDoUI
@using ShiftSoftware.ShiftBlazor.Components
@using ShiftSoftware.ShiftBlazor.Utils
@using ShiftSoftware.ShiftBlazor.Services
@using MudBlazor
```

Then, inside ``wwwroot/index.html``, add the following codes.

``` html hl_lines="9-11 26 27"
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="utf-8" />
    <title>ToDoUI</title>
    <base href="/" />
    <link href="css/app.css" rel="stylesheet" />
    <link href="_content/MudBlazor/MudBlazor.min.css" rel="stylesheet" />
    <link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>

    <!-- If you add any scoped CSS files, uncomment the following to load them
    <link href="ToDoUI.styles.css" rel="stylesheet" /> -->
</head>

<body>
    <div id="app">Loading...</div>

    <div id="blazor-error-ui">
        An unhandled error has occurred.
        <a href="" class="reload">Reload</a>
        <a class="dismiss">🗙</a>
    </div>
    <script src="_framework/blazor.webassembly.js"></script>
    <script src="_content/MudBlazor/MudBlazor.min.js"></script>
    <script src="_content/ShiftSoftware.ShiftBlazor/main.js"></script>
</body>
</html>
```

## Connecting The ToDoAPI Project With ToDoUI

Inside the ToDoAPI project in Visual Studio, right click on the project solution and select ``New -> Project``. Then, create a new ``Class Library`` project and name it ``Shared``.

``Shared`` project will be used for sharing the DTO classes with the Blazor Project. Therefore, copy ``DTOs`` folder and paste it inside the ``Shared`` project.

Now, we need to connect the two project through the ``Shared`` project. Inside the ``ToDoAPI`` Project, right click on ``Dependencies``, select ``Add Project Reference...`` and Tick the ``Shared`` project. Follow the same process for the ``ToDoUI`` project.

Then, go to ``_imports.razor`` and add the folders of the DTOs that we will use for this project like the following.

``` razor hl_lines="12"
@using System.Net.Http
@using System.Net.Http.Json
@using Microsoft.AspNetCore.Components.Routing
@using Microsoft.AspNetCore.Components.Web
@using Microsoft.AspNetCore.Components.WebAssembly.Http
@using Microsoft.JSInterop
@using ToDoUI
@using ShiftSoftware.ShiftBlazor.Components
@using ShiftSoftware.ShiftBlazor.Utils
@using ShiftSoftware.ShiftBlazor.Services
@using MudBlazor
@using ToDoAPI.DTOs.ToDo
```

Lastly, add the API URL to the HTTP client in ``Program.cs``.

``` cs hl_lines="5"
var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient { BaseAddress = new Uri("Place Your API URL Here") });
builder.Services.AddShiftServices(config => { });

await builder.Build().RunAsync();
```