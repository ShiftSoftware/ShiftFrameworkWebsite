# Adding the Controllers

``` hl_lines="2"
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
│   │   │   ├── ToDoRepository.cs
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

## Adding The Controller to Our API

Inside **Controllers** folder, add a new class and name it ``ToDoController.cs``.

``` hl_lines="7"
.
├── ToDo.API
│   ├── Connected Services
│   ├── Dependencies
│   ├── Properties
│   ├── Controllers
│   │   ├── ToDoController.cs
│   ├── Data
│   │   ├── Entities
│   │   │   ├── ToDo.cs
│   │   ├── Repositories
│   │   │   ├── ToDoRepository.cs
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

Then, add the route to the controller.

``` cs hl_lines="5"
using Microsoft.AspNetCore.Mvc;

namespace ToDo.API.Controllers
{
    [Route("api/[controller]")]
    public class ToDoController
    {
    }
}
```

After that, add ``ShiftEntityControllerAsync`` to the class and write the constructor.

``` cs hl_lines="9 11-13"
using Microsoft.AspNetCore.Mvc;
using ShiftSoftware.ShiftEntity.Web;
using ToDo.API.Data.Repositories;
using ToDo.Shared.DTOs.ToDo;

namespace ToDo.API.Controllers
{
    [Route("api/[controller]")]
    public class ToDoController : ShiftEntityControllerAsync<ToDoRepository, Data.Entities.ToDo, ToDoListDTO, ToDoDTO>
    {
        public ToDoController(ToDoRepository repository) : base(repository)
        {
        }
    }
}
```