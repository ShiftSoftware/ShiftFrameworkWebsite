# Using Shift Entities

``` hl_lines="2"
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

## Installing The Packages

Go to NuGet Package Manager and install the following two packages:

1. ``ShiftSoftware.ShiftEntity.Web``
2. ``Microsoft.EntityFrameworkCore.Tools``

## Adding Entities To Our API

Right-click on **Data/Entities** folder and add a new class named ``ToDo.cs``.

``` hl_lines="9"
.
├── ToDo.API
│   ├── Connected Services
│   ├── Dependencies
│   ├── Properties
│   ├── Controllers
│   ├── Data
│   │   ├── Entities
│   │   │   ├── ToDo.cs
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

Then, add the following properties to the class:

``` cs hl_lines="7-9"
using ToDo.Shared.Enums;

namespace ToDo.API.Data.Entities;

public class ToDo
{
    public string Title { get; set; } = default!;
    public string Description { get; set; } = default!;
    public ToDoStatus Status { get; set; }
}
```

After that, add *ShiftEntity* and *TemporalShiftEntity* to the class.

``` cs hl_lines="6-7"
using ShiftSoftware.ShiftEntity.Core;
using ToDo.Shared.Enums;

namespace ToDo.API.Data.Entities;

[TemporalShiftEntity]
public class ToDo : ShiftEntity<ToDo>
{
    public string Title { get; set; } = default!;
    public string Description { get; set; } = default!;
    public ToDoStatus Status { get; set; }
}
```

Lastly, add the following two operators to the class:

``` cs hl_lines="14-31 33-46"
using ShiftSoftware.ShiftEntity.Core;
using ToDo.Shared.DTOs.ToDo;
using ToDo.Shared.Enums;

namespace ToDo.API.Data.Entities;

[TemporalShiftEntity]
public class ToDo : ShiftEntity<ToDo>
{
    public string Title { get; set; } = default!;
    public string Description { get; set; } = default!;
    public ToDoStatus Status { get; set; }

    public static implicit operator ToDoDTO(ToDo entity)
    {
        if (entity == null)
            return default!;

        return new ToDoDTO {
            CreateDate = entity.CreateDate,
            CreatedByUserID = entity.CreatedByUserID,
            IsDeleted = entity.IsDeleted,
            LastSaveDate = entity.LastSaveDate,
            LastSavedByUserID = entity.LastSavedByUserID,
            ID = entity.ID,

            Description = entity.Description,
            Status = entity.Status,
            Title = entity.Title,
        };
    }

    public static implicit operator ToDoListDTO(ToDo entity)
    {
        if (entity == null)
            return default!;

        return new ToDoListDTO
        {
            ID = entity.ID,

            Description = entity.Description,
            Status = entity.Status,
            Title = entity.Title,
        };
    }
}
```

## Adding The Database

Inside **Data** folder add a new class and name it ``DB.cs``.

``` hl_lines="11"
.
├── ToDo.API
│   ├── Connected Services
│   ├── Dependencies
│   ├── Properties
│   ├── Controllers
│   ├── Data
│   │   ├── Entities
│   │   │   ├── ToDo.cs
│   │   ├── Repositories
│   │   ├── DB.cs
│   ├── appsettings.json
│   ├── Program.cs
│
├── ToDo.Shared
│
├── ToDo.Test
│
├── ToDo.Web
```

Then, add ``ShiftDbConetxt`` to the class and write the constructor.

``` cs hl_lines="6 8-10"
using Microsoft.EntityFrameworkCore;
using ShiftSoftware.ShiftEntity.Core;

namespace ToDo.API.Data
{
    public class DB : ShiftDbContext
    {
        public DB(DbContextOptions option) : base(option)
        {
        }
    }
}
```

After that, add the following property:

``` cs hl_lines="12"
using Microsoft.EntityFrameworkCore;
using ShiftSoftware.ShiftEntity.Core;

namespace ToDo.API.Data
{
    public class DB : ShiftDbContext
    {
        public DB(DbContextOptions option) : base(option)
        {
        }
        
        public DbSet<Entities.ToDo> ToDos { get; set; }
    }
}
```