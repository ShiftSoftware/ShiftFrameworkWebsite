# Creating a List to Show Our ToDos

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

## Add Navigation Button to New Page

First, create a new folder inside **Pages** folder, and name it ``_ToDo``. After that, create a new razor component in the folder and name it ``ToDoList.razor``.

``` hl_lines="17-18"
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
│   │   ├── _ToDo
│   │   │   ├── ToDoList.razor
│   ├── Services
│   ├── Shared
│   │   ├── MainLayout.razor
│   │   ├── NavMenu.razor
│   ├── _Imports.razor
│   ├── App.razor
│   ├── Program.cs
```

Then, go inside **NavMenu.razor** and add the below codes:

``` razor hl_lines="1 3"
@using ToDo.Web.Pages._ToDo;
<MudNavMenu>
    <MudNavLink Href="@($"/{nameof(ToDoList)}")" Match="NavLinkMatch.All" Icon="@Icons.Material.Outlined.Task">To Do</MudNavLink>
</MudNavMenu>

@code {
    [Parameter] public bool isDrawerOpen { get; set; }
}
```

## Adding ShiftList

Inisde **ToDoList.razor** file, delete the whole code and write the following code:

``` razor
@attribute [Route($"/{nameof(ToDoList)}")]

@using Syncfusion.Blazor.Grids
@using ToDo.Shared.DTOs.ToDo

<ShiftList Action="/ToDo"
           Title="ToDo List"
           T="ToDoListDTO"
           EnablePdfExport
           AutoGenerateColumns="false"
           EnableCsvExcelExport>
    <ColumnTemplate>
        <GridColumn HeaderText="Title" Field="@nameof(ToDoListDTO.Title)" />
        <GridColumn HeaderText="Description" Field="@nameof(ToDoListDTO.Description)" />
    </ColumnTemplate>
</ShiftList>
```