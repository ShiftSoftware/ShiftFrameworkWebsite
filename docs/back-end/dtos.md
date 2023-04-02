# Adding DTOs to ToDo.Shared

``` hl_lines="4"
ToDo
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

## Installing The Packages

Open NuGet Package Manager for the **ToDo.Shared** project and install ``ShiftSoftware.ShiftEntity.Model`` package.

## Creating The Classes

First, go to **ToDo.Shared** project and right-click on **DTOs** folder, select ``Add -> New Folder`` and name it ``ToDo``. Inside **ToDo** folder, add a class by right-clicking on the **ToDo** folder and name it ``ToDoDTO.cs``. Then, inside the same folder, add another class and name it ``ToDoListDTO.cs``.

``` hl_lines="7-9"
ToDo
├── ToDo.API
│
├── ToDo.Shared
│   ├── Dependencies
│   ├── DTOs
│   │   ├── ToDo
│   │   │   ├── ToDoDTO.cs
│   │   │   ├── ToDoListDTO.cs
│   ├── Enums
│
├── ToDo.Test
│
├── ToDo.Web
```

After that, inside the **Enums** folder, add a class and name it ``ToDoStatus.cs``.

``` hl_lines="7-8"
ToDo
├── ToDo.API
│
├── ToDo.Shared
│   ├── Dependencies
│   ├── DTOs
│   ├── Enums
│   │   ├── ToDoStatus.cs
│
├── ToDo.Test
│
├── ToDo.Web
```

In the class, change the *class* to *enum* and *internal* to *public* like the following example:

``` cs hl_lines="3"
namespace ToDo.Shared.Enums;

public enum ToDoStatus
{
}
```

Change *internal* to *public* inside the DTOs too.

**ToDoDTO.cs**:

``` cs hl_lines="3"
namespace ToDo.Shared.DTOs.ToDo;

public class ToDoDTO
{
}
```

**ToDoListDTO.cs**:

``` cs hl_lines="3"
namespace ToDo.Shared.DTOs.ToDo;

public class ToDoListDTO
{
}
```

## Adding Properties to Our Classes

Inside **ToDoStatus.cs** add the follwoing enums:

``` cs hl_lines="5-7"
namespace ToDo.Shared.Enums;

public enum ToDoStatus
{
    New = 1,
    InProgress = 2,
    Completed = 3
}
```

After that, inside **ToDoDTO.cs** add the following properties to the class:

``` cs hl_lines="9-13"
using ShiftSoftware.ShiftEntity.Model.Dtos;
using System.Text.Json.Serialization;
using ToDo.Shared.Enums;

namespace ToDo.Shared.DTOs.ToDo;

public class ToDoDTO
{
    public string Title { get; set; } = default!;
    public string Description { get; set; } = default!;

    [JsonConverter(typeof(JsonStringEnumConverter))]
    public ToDoStatus Status { get; set; }
}
```

Then, inside **ToDoListDTO.cs** add the following properties to the class:

``` cs hl_lines="9-13"
using ShiftSoftware.ShiftEntity.Model.Dtos;
using System.Text.Json.Serialization;
using ToDo.Shared.Enums;

namespace ToDo.Shared.DTOs.ToDo;

public class ToDoListDTO
{
    public string Title { get; set; } = default!;
    public string Description { get; set; } = default!;

    [JsonConverter(typeof(JsonStringEnumConverter))]
    public ToDoStatus Status { get; set; }
}
```

## Adding Shift Entity DTO to Our Classes

First, go to NuGet Package Manager and install the latest version of ``ShiftSoftware.ShiftEntity.Model``.

Then, go to **ToDoDTO.cs** and add the following code:

``` cs hl_lines="7"
using ShiftSoftware.ShiftEntity.Model.Dtos;
using System.Text.Json.Serialization;
using ToDo.Shared.Enums;

namespace ToDo.Shared.DTOs.ToDo;

public class ToDoDTO : ShiftEntityDTO
{
    public string Title { get; set; } = default!;
    public string Description { get; set; } = default!;

    [JsonConverter(typeof(JsonStringEnumConverter))]
    public ToDoStatus Status { get; set; }
}
```

Inside **ToDoListDTO.cs** add the following code:

``` cs hl_lines="7"
using ShiftSoftware.ShiftEntity.Model.Dtos;
using System.Text.Json.Serialization;
using ToDo.Shared.Enums;

namespace ToDo.Shared.DTOs.ToDo;

public class ToDoListDTO : ShiftEntityListDTO
{
    public string Title { get; set; } = default!;
    public string Description { get; set; } = default!;

    [JsonConverter(typeof(JsonStringEnumConverter))]
    public ToDoStatus Status { get; set; }
}
```