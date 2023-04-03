# Getting Started with Shift Framework for Front-end Development

``` hl_lines="8"
ToDo
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

## Installing The Packages

Open NuGet Package Manager and install ``ShiftSoftware.ShiftBlazor`` package for the project.

??? Note "If the search result was empty."

    Make sure you have ticked **Include prerelease** option next to the search field.

## Setting Up The Project

### Adding appsettings.Development.json

First, add a new item inside **wwwroot** folder and name it ``appsettings.Development.json``.

``` hl_lines="14"
ToDo
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
│   │   ├── css
│   │   ├── appsettings.Development.json
│   │   ├── index.html
│   ├── Pages
│   ├── Services
│   ├── Shared
│   ├── _Imports.razor
│   ├── App.razor
│   ├── MainLayout.razor
│   ├── Program.cs
```

Inside the file, add the below code:

``` json
{
  "SyncfusionLicense": "[Add your sync fusion license here]",
  "BaseURL": "[Add your base URL here]"
}
```

??? hint "If you want to find your base URL, follow these steps."

    Go to **launchSettings.json** inside **ToDo.API/Properties**.

    ``` hl_lines="6"
    ToDo
    ├── ToDo.API
    │   ├── Connected Services
    │   ├── Dependencies
    │   ├── Properties
    │   │   ├── launchSettings.json
    │   ├── Controllers
    │   │   ├── ToDoController.cs
    │   ├── Data
    │   │   ├── Entities
    │   │   │   ├── ToDo.cs
    │   │   ├── Repositories
    │   │   │   ├── ToDoRepository.cs
    │   │   ├── DB.cs
    │   ├── appsettings.json
    │   │   ├── appsettings.Development.json
    │   ├── Program.cs
    │
    ├── ToDo.Shared
    │
    ├── ToDo.Test
    │
    ├── ToDo.Web
    ```

    You can find the *BaseURL* value in the following code:

    ``` json hl_lines="15"
    {
        "iisSettings": {
            "windowsAuthentication": false,
            "anonymousAuthentication": true,
            "iisExpress": {
                "applicationUrl": "http://localhost:22171",
                "sslPort": 0
            }
        },
        "profiles": {
            "http": {
                "commandName": "Project",
                "dotnetRunMessages": true,
                "launchBrowser": true,
                "applicationUrl": "http://localhost:5028",
                "environmentVariables": {
                    "ASPNETCORE_ENVIRONMENT": "Development"
                }
            },
            "IIS Express": {
                "commandName": "IISExpress",
                "launchBrowser": true,
                "environmentVariables": {
                    "ASPNETCORE_ENVIRONMENT": "Development"
                }
            }
        }
    }
    ```

### Seting Up index.html

Add the following codes to your **index.html** file and replace *body* element with the *body* of the example below:

``` html hl_lines="6 11-16 19-36"
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
    <title>ToDo.Web</title>
    <base href="/" />
    <link href="css/app.css" rel="stylesheet" />

    <link href="ToDo.Web.styles.css" rel="stylesheet" />
    <link href="_content/ShiftSoftware.ShiftBlazor/main.css" rel="stylesheet" />
    <link href="https://fonts.googleapis.com/css?family=Roboto:300,400,500,700&display=swap" rel="stylesheet" />
    <link href="_content/MudBlazor/MudBlazor.min.css" rel="stylesheet" />
    <link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</head>

<body>
    <div id="app">
        <svg class="loading-progress">
            <circle r="40%" cx="50%" cy="50%" />
            <circle r="40%" cx="50%" cy="50%" />
        </svg>
        <div class="loading-progress-text"></div>
    </div>

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

### Setting Up app.css

Go to **app.css** and replace the below code with the code inside the file.

``` css
input-table .mud-input-control mud-select {
    margin: 0 !important;
}

.input-table .mud-input-control > .mud-input-control-input-container > div.mud-input.mud-input-text {
    margin-top: 0 !important;
}

.input-table .mud-input-control {
    margin-top: 0 !important;
}

.input-table .mud-simple-table.mud-table-bordered .mud-table-container table tbody tr td {
    padding: 5px !important
}

.grey-expansion-headers .mud-expand-panel .mud-expand-panel-header {
    background-color: var(--mud-palette-table-striped);
    font-size: 20px;
}

.grey-expansion-headers .mud-expand-panel .mud-expand-panel-content {
    padding: 16px 24px 16px;
}

.light-toolbar, .mud-tabs.light-toolbar .mud-tabs-toolbar {
    background-color: var(--mud-palette-table-striped);
    border: 1px solid var(--mud-palette-table-lines);
    border-top-left-radius: 3px;
    border-top-right-radius: 3px;
}

.mud-tabs.light-toolbar .mud-tabs-panels {
    background: var(--mud-palette-surface);
}

.mud-drawer-content > .mud-navmenu .mud-nav-link .mud-icon-root:first-child + .mud-nav-link-text {
    transition: 0.3s;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}

.mud-drawer--closed.mud-drawer-mini > .mud-drawer-content > .mud-navmenu .mud-nav-link .mud-icon-root:first-child + .mud-nav-link-text {
    display: initial !important;
    visibility: hidden;
    opacity: 0;
}

html, body {
    font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
}

h1:focus {
    outline: none;
}

a, .btn-link {
    color: #0071c1;
}

.btn-primary {
    color: #fff;
    background-color: #1b6ec2;
    border-color: #1861ac;
}

.btn:focus, .btn:active:focus, .btn-link.nav-link:focus, .form-control:focus, .form-check-input:focus {
    box-shadow: 0 0 0 0.1rem white, 0 0 0 0.25rem #258cfb;
}

.content {
    padding-top: 1.1rem;
}

.valid.modified:not([type=checkbox]) {
    outline: 1px solid #26b050;
}

.invalid {
    outline: 1px solid red;
}

.validation-message {
    color: red;
}

#blazor-error-ui {
    background: lightyellow;
    bottom: 0;
    box-shadow: 0 -1px 2px rgba(0, 0, 0, 0.2);
    display: none;
    left: 0;
    padding: 0.6rem 1.25rem 0.7rem 1.25rem;
    position: fixed;
    width: 100%;
    z-index: 1000;
}

    #blazor-error-ui .dismiss {
        cursor: pointer;
        position: absolute;
        right: 0.75rem;
        top: 0.5rem;
    }

.blazor-error-boundary {
    background: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNTYiIGhlaWdodD0iNDkiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIgeG1sbnM6eGxpbms9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiIG92ZXJmbG93PSJoaWRkZW4iPjxkZWZzPjxjbGlwUGF0aCBpZD0iY2xpcDAiPjxyZWN0IHg9IjIzNSIgeT0iNTEiIHdpZHRoPSI1NiIgaGVpZ2h0PSI0OSIvPjwvY2xpcFBhdGg+PC9kZWZzPjxnIGNsaXAtcGF0aD0idXJsKCNjbGlwMCkiIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0yMzUgLTUxKSI+PHBhdGggZD0iTTI2My41MDYgNTFDMjY0LjcxNyA1MSAyNjUuODEzIDUxLjQ4MzcgMjY2LjYwNiA1Mi4yNjU4TDI2Ny4wNTIgNTIuNzk4NyAyNjcuNTM5IDUzLjYyODMgMjkwLjE4NSA5Mi4xODMxIDI5MC41NDUgOTIuNzk1IDI5MC42NTYgOTIuOTk2QzI5MC44NzcgOTMuNTEzIDI5MSA5NC4wODE1IDI5MSA5NC42NzgyIDI5MSA5Ny4wNjUxIDI4OS4wMzggOTkgMjg2LjYxNyA5OUwyNDAuMzgzIDk5QzIzNy45NjMgOTkgMjM2IDk3LjA2NTEgMjM2IDk0LjY3ODIgMjM2IDk0LjM3OTkgMjM2LjAzMSA5NC4wODg2IDIzNi4wODkgOTMuODA3MkwyMzYuMzM4IDkzLjAxNjIgMjM2Ljg1OCA5Mi4xMzE0IDI1OS40NzMgNTMuNjI5NCAyNTkuOTYxIDUyLjc5ODUgMjYwLjQwNyA1Mi4yNjU4QzI2MS4yIDUxLjQ4MzcgMjYyLjI5NiA1MSAyNjMuNTA2IDUxWk0yNjMuNTg2IDY2LjAxODNDMjYwLjczNyA2Ni4wMTgzIDI1OS4zMTMgNjcuMTI0NSAyNTkuMzEzIDY5LjMzNyAyNTkuMzEzIDY5LjYxMDIgMjU5LjMzMiA2OS44NjA4IDI1OS4zNzEgNzAuMDg4N0wyNjEuNzk1IDg0LjAxNjEgMjY1LjM4IDg0LjAxNjEgMjY3LjgyMSA2OS43NDc1QzI2Ny44NiA2OS43MzA5IDI2Ny44NzkgNjkuNTg3NyAyNjcuODc5IDY5LjMxNzkgMjY3Ljg3OSA2Ny4xMTgyIDI2Ni40NDggNjYuMDE4MyAyNjMuNTg2IDY2LjAxODNaTTI2My41NzYgODYuMDU0N0MyNjEuMDQ5IDg2LjA1NDcgMjU5Ljc4NiA4Ny4zMDA1IDI1OS43ODYgODkuNzkyMSAyNTkuNzg2IDkyLjI4MzcgMjYxLjA0OSA5My41Mjk1IDI2My41NzYgOTMuNTI5NSAyNjYuMTE2IDkzLjUyOTUgMjY3LjM4NyA5Mi4yODM3IDI2Ny4zODcgODkuNzkyMSAyNjcuMzg3IDg3LjMwMDUgMjY2LjExNiA4Ni4wNTQ3IDI2My41NzYgODYuMDU0N1oiIGZpbGw9IiNGRkU1MDAiIGZpbGwtcnVsZT0iZXZlbm9kZCIvPjwvZz48L3N2Zz4=) no-repeat 1rem/1.8rem, #b32121;
    padding: 1rem 1rem 1rem 3.7rem;
    color: white;
}

    .blazor-error-boundary::after {
        content: "An error has occurred."
    }

.loading-progress {
    position: relative;
    display: block;
    width: 8rem;
    height: 8rem;
    margin: 20vh auto 1rem auto;
}

    .loading-progress circle {
        fill: none;
        stroke: #e0e0e0;
        stroke-width: 0.6rem;
        transform-origin: 50% 50%;
        transform: rotate(-90deg);
    }

        .loading-progress circle:last-child {
            stroke: #1b6ec2;
            stroke-dasharray: calc(3.141 * var(--blazor-load-percentage, 0%) * 0.8), 500%;
            transition: stroke-dasharray 0.05s ease-in-out;
        }

.loading-progress-text {
    position: absolute;
    text-align: center;
    font-weight: bold;
    inset: calc(20vh + 3.25rem) 0 auto 0.2rem;
}

    .loading-progress-text:after {
        content: var(--blazor-load-percentage-text, "Loading");
    }
```

### Setting Up MainLayout.razor

First, delete **MainLayout.razor** inside the **ToDo.Web**.

``` hl_lines="21"
ToDo
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
│   │   ├── css
│   │   ├── appsettings.Development.json
│   │   ├── index.html
│   ├── Pages
│   ├── Services
│   ├── Shared
│   ├── _Imports.razor
│   ├── App.razor
│   ├── MainLayout.razor
│   ├── Program.cs
```

After that, inside **Shared** folder, add a new razor component and name it ``MainLayout.razor``.

``` hl_lines="19"
ToDo
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
│   │   ├── css
│   │   ├── appsettings.Development.json
│   │   ├── index.html
│   ├── Pages
│   ├── Services
│   ├── Shared
│   │   ├── MainLayout.razor
│   ├── _Imports.razor
│   ├── App.razor
│   ├── Program.cs
```

Then, write the following code inside the component:

``` razor
@inherits ShiftMainLayout
@inject NavigationManager navigationManager

<AddMudProviders/>

<MudLayout>

    <MudAppBar>
        <MudIconButton Icon="@Icons.Material.Filled.Menu" Color="Color.Inherit" Edge="Edge.Start" OnClick="@((e) => DrawerToggle())" />
        <MudLink Href="/"><MudText Style="color: #fff;">ToDo App</MudText></MudLink>

        <MudSpacer />
    </MudAppBar>

    <MudDrawer @bind-Open="@_drawerOpen" ClipMode="DrawerClipMode.Docked" Elevation="1" Variant="@DrawerVariant.Mini" Breakpoint="Breakpoint.Lg" OpenMiniOnHover="true">
        <NavMenu isDrawerOpen="_drawerOpen" />
    </MudDrawer>

    <MudMainContent>
        <MudContainer MaxWidth="MaxWidth.ExtraLarge" Style="padding-block: 24px;">
            @Body
        </MudContainer>
    </MudMainContent>

</MudLayout>

@code {
    bool _drawerOpen = false;

    void DrawerToggle()
    {
        _drawerOpen = !_drawerOpen;
    }
}
```

### Setting Up NavMenu.razor

Inside **Shared** folder, add a new razor component and name it ``NavMenu.razor``.

``` hl_lines="20"
ToDo
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
│   │   ├── css
│   │   ├── appsettings.Development.json
│   │   ├── index.html
│   ├── Pages
│   ├── Services
│   ├── Shared
│   │   ├── MainLayout.razor
│   │   ├── NavMenu.razor
│   ├── _Imports.razor
│   ├── App.razor
│   ├── Program.cs
```

Then, write the following code inside it:

``` razor
<MudNavMenu>
</MudNavMenu>

@code {
    [Parameter] public bool isDrawerOpen { get; set; }
}
```

### Setting Up _Imports.razor

Go to **_Imports.razor** and replace the the code inside it with the following code:

``` razor
@using System.Net.Http
@using System.Net.Http.Json
@using Microsoft.AspNetCore.Components.Forms
@using Microsoft.AspNetCore.Components.Routing
@using Microsoft.AspNetCore.Components.Web
@using Microsoft.AspNetCore.Components.Web.Virtualization
@using Microsoft.AspNetCore.Components.WebAssembly.Http
@using Microsoft.JSInterop
@using ToDo.Web
@using ToDo.Web.Shared
@using MudBlazor
@using ShiftSoftware.ShiftBlazor.Components
@using ShiftSoftware.ShiftBlazor.Utils
```

### Setting Up App.razor

Go to **App.razor** and replace the the code inside it with the following code:

``` razor
<Router AppAssembly="@typeof(App).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
        <FocusOnNavigate RouteData="@routeData" Selector="h1" />
    </Found>
    <NotFound>
        <PageTitle>Not found</PageTitle>
        <LayoutView Layout="@typeof(MainLayout)">
            <p role="alert">Sorry, there's nothing at this address.</p>
        </LayoutView>
    </NotFound>
</Router>
```

### Setting Up Program.cs

Inside **Program.cs**, replace the following line of code:

``` cs hl_lines="9"
using Microsoft.AspNetCore.Components.Web;
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using ToDo.Web;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient { BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) });

await builder.Build().RunAsync();
```

With this code:

``` cs hl_lines="9-12 14"
using Microsoft.AspNetCore.Components.Web;
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using ToDo.Web;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

var httpClient = new HttpClient
{
    BaseAddress = new Uri(builder.Configuration!.GetValue<string>("BaseURL")!),
};

builder.Services.AddScoped(sp => httpClient);

await builder.Build().RunAsync();
```

After that, add Shift Services.

``` cs hl_lines="17-27"
using Microsoft.AspNetCore.Components.Web;
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using ShiftSoftware.ShiftBlazor.Extensions;
using ToDo.Web;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

var httpClient = new HttpClient
{
    BaseAddress = new Uri(builder.Configuration!.GetValue<string>("BaseURL")!),
};

builder.Services.AddScoped(sp => httpClient);

builder.Services.AddShiftServices(config =>
{
    config.ShiftConfiguration = options =>
    {
        options.BaseAddress = builder.Configuration!.GetValue<string>("BaseURL")!;
        options.ApiPath = "/api";
        options.ODataPath = "/odata";
        options.UserListEndpoint = "http://localhost";
    };
    config.SyncfusionLicense = builder.Configuration.GetValue<string>("SyncfusionLicense");
});

await builder.Build().RunAsync();
```