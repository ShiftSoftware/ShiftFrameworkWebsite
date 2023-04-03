# Creating a Form For Working With The Data

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

## Adding a Form

First, create a new razor component in the **_ToDo** folder and name it ``ToDoForm.razor``.

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
│   │   ├── _ToDo
│   │   │   ├── ToDoList.razor
│   │   │   ├── ToDoForm.razor
│   ├── Services
│   ├── Shared
│   │   ├── MainLayout.razor
│   │   ├── NavMenu.razor
│   ├── _Imports.razor
│   ├── App.razor
│   ├── Program.cs
```

Then, inisde **ToDoForm.razor** file, delete the whole code and write the following code:

``` razor
@attribute [Route($"/{nameof(ToDoForm)}/{{Key?}}")]

@inherits ShiftForm<ToDoForm, ToDo.Shared.DTOs.ToDo.ToDoDTO>

<ShiftEntityForm @bind-Mode="Mode"
                 @bind-Value="TheItem"
                 @ref="FormContainer"
                 Action="ToDo"
                 Title="ToDo"
                 Settings="@FormSetting"
                 @bind-Key="@Key"
                 T="ToDo.Shared.DTOs.ToDo.ToDoDTO">

    <MudTextField ReadOnly="@ReadOnly"
                  Disabled="@Disabled"
                  OnlyValidateIfDirty="true"
                  Label="Title"
                  @bind-Value="TheItem.Title"
                  For="@(() => TheItem.Title)"
                  Immediate="true" />

    <MudTextField ReadOnly="@ReadOnly"
                  Disabled="@Disabled"
                  OnlyValidateIfDirty="true"
                  Immediate="true"
                  Label="Description"
                  @bind-Value="TheItem.Description"
                  For="@(() => TheItem.Description)" />

    <MudSelect ReadOnly="@ReadOnly"
               Disabled="@Disabled"
               OnlyValidateIfDirty="true"
               T="ToDo.Shared.Enums.ToDoStatus"
               Label="Status"
               AnchorOrigin="Origin.BottomCenter"
               @bind-Value="TheItem.Status"
               For="@(() => TheItem.Status)">
        <MudSelectItem Value="@(ToDo.Shared.Enums.ToDoStatus.New)" />
        <MudSelectItem Value="@(ToDo.Shared.Enums.ToDoStatus.InProgress)" />
        <MudSelectItem Value="@(ToDo.Shared.Enums.ToDoStatus.Completed)" />
    </MudSelect>
</ShiftEntityForm>
```