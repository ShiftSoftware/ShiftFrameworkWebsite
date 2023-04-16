# Printing One ToDo

``` hl_lines="2"
ToDo
├── ToDo.API
│   ├── Connected Services
│   ├── Dependencies
│   ├── Properties
│   ├── Controllers
│   ├── Data
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

## Pre-requisites

In order to design the PDF template that we want to print, we need to install the Fast Print Designer. You can download it from this [link](https://fastreports.github.io/FastReport.Documentation/FastReportOnlineDesigner.html).

## Using Fast Print Designer

After installing the Fast Print Designer, we can start designing the template that we want to print. For the purpose of this tutorial, we will not be going into details on how to use Fast Print Designer.

First, open the Fast Print Designer, and add a new table as the following example:

|       |  |
| ----------- | ----------- |
| ID      | [Task.ID]       |
| Title   | [Task.Title]        |
| Description      | [Task.Description]       |
| Status   | [Task.Status]        |

Second, select all the data fields (All rows of the second column). Then, go to **Preoperties -> Data** and set **AllowExperession** to ``False``.

Third, we go to the **ToDo.API** project and add a new folder called ``Reports``. Then, save the file inside that folder as ``Task.frx``.

``` hl_lines="8-9"
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

## Adding One Task to Print Out

First, go to **ToDo.API -> Controllers -> ToDoController.cs**. Then, add the following route to the class:

``` cs hl_lines="19-22"
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
        public ToDoController(ToDoRepository repository) : base(repository)
        {
        }

        [HttpGet("print/{ID}")]
        public async Task<ActionResult> Print(long ID)
        {
        }
    }
}
```

Second, find the ToDo by the ID and pass the needed attributes to a new variable called ``data``:

``` cs hl_lines="22-30"
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
        public ToDoController(ToDoRepository repository) : base(repository)
        {
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
        }
    }
}
```

Third, return the PDF file:

``` cs hl_lines="32-35"
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
        public ToDoController(ToDoRepository repository) : base(repository)
        {
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