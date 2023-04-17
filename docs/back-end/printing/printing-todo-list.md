# Printing a List of ToDos

``` hl_lines="2"
ToDo
├── ToDo.API
│   ├── Connected Services
│   ├── Dependencies
│   ├── Properties
│   ├── Controllers
│   ├── Data
│   ├── Reports
│   │   ├── Task.frx
│   ├── appsettings.json
│   ├── Program.cs
│   ├── WebMarker.cs
│
├── ToDo.Shared
│
├── ToDo.Test
│
├── ToDo.Web
```

## Editing The PDF Template

First, open the PDF template inside FastReport Designer. Then, add a new table as the following example:

| ID | Title | Description | Status |
| ----------- | ----------- | ----------- | ----------- |
| [AllTasks.ID] | [AllTasks.Title] | [AllTasks.Description] | [AllTasks.Status] |

!!! note

    The second row (The data fields that you want it to be repeated) need to be placed inside a **Data Band**. If it is not placed inside it, it will only show one row instead of a list.

Second, select all the data fields (Second row of the table). Then, go to **Preoperties -> Data** and set **AllowExperession** to `False`.

Third, select the **Data Band** and then go to **Properties -> Design**. Set **(Name)** to ``AllTasksDataBand``.

## Editing The Print Route

First, go to the **ToDo.API -> Controllers -> ToDoController.cs** file. Then, add the database to the class.

``` hl_lines="9"
ToDo
├── ToDo.API
│   ├── Connected Services
│   ├── Dependencies
│   ├── Properties
│   ├── Controllers
│   ├── Data
│   ├── Reports
│   │   ├── Task.frx
│   ├── appsettings.json
│   ├── Program.cs
│   ├── WebMarker.cs
│
├── ToDo.Shared
│
├── ToDo.Test
│
├── ToDo.Web
```

``` cs hl_lines="15-16 18"
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;
using MudBlazor.Extensions;
using ShiftSoftware.ShiftEntity.Web;
using ShiftSoftware.ShiftEntity.Web.Services;
using ToDo.API.Data;
using ToDo.API.Data.Repositories;
using ToDo.Shared.DTOs.ToDo;

namespace ToDo.API.Controllers
{
    [Route("api/[controller]")]
    public class ToDoController : ShiftEntityControllerAsync<ToDoRepository, Data.Entities.ToDo, ToDoListDTO, ToDoDTO>
    {
        private DB db;
        public ToDoController(ToDoRepository repository, DB db) : base(repository)
        {
            this.db = db;
        }

        [HttpGet("print/{ID}")]
        public async Task<ActionResult> Print(long ID)
        {
            var task = await base.repository.FindAsync(ID);

            var data = new
            {
                ID = task.ID,
                Title = task.Title,
                Description = task.Description,
                Status = task.Status.ToDescriptionString(),
            };

            return await new FastReportBuilder()
                .AddFastReportFile("Reports/Task.frx")
                .AddDataObject("Task", data)
                .GetPDFFile();
        }
    }
}
```

Second, fetch the list of ToDos from the database and return it inside the PDF file.

``` cs hl_lines="34-45 50"
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;
using MudBlazor.Extensions;
using ShiftSoftware.ShiftEntity.Web;
using ShiftSoftware.ShiftEntity.Web.Services;
using ToDo.API.Data;
using ToDo.API.Data.Repositories;
using ToDo.Shared.DTOs.ToDo;

namespace ToDo.API.Controllers
{
    [Route("api/[controller]")]
    public class ToDoController : ShiftEntityControllerAsync<ToDoRepository, Data.Entities.ToDo, ToDoListDTO, ToDoDTO>
    {
        private DB db;
        public ToDoController(ToDoRepository repository, DB db) : base(repository)
        {
            this.db = db;
        }

        [HttpGet("print/{ID}")]
        public async Task<ActionResult> Print(long ID)
        {
            var task = await base.repository.FindAsync(ID);

            var data = new
            {
                ID = task.ID,
                Title = task.Title,
                Description = task.Description,
                Status = task.Status.ToDescriptionString(),
            };

            var allTasks = (await this.db
                .ToDos
                .Where(x => x.ID != task.ID)
                .ToListAsync())
                .Select(x => new
                {
                    ID = x.ID,
                    Title = x.Title,
                    Description = x.Description,
                    Status = x.Status.ToDescriptionString(),
                })
                .ToList();

            return await new FastReportBuilder()
                .AddFastReportFile("Reports/Task.frx")
                .AddDataObject("Task", data)
                .AddDataList("AllTasks", "AllTasksDataBand", allTasks.ToList<object>())
                .GetPDFFile();
        }
    }
}
```