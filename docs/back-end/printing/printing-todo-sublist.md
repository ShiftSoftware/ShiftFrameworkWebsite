# Printing a Sub List of ToDos

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

First, open the PDF template inside Fast Print Designer. Then, add a new table as the following example:

| [Statuses.Name] | | | |
| ----------- | ----------- | ----------- | ----------- |
| [AllTasks.ID] | [AllTasks.Title] | [AllTasks.Description] | [AllTasks.Status] |

!!! note

    In order for add the sub list, when you create the column data, right-click on **Data** and select **Add Detail Data Band**. After that, add the row data inside the **Sub Data** Field.

Second, select all the data fields. Then, go to **Preoperties -> Data** and set **AllowExperession** to `False`.

Third, select **Data** of the first row and then go to **Properties -> Design**. Set **(Name)** to ``StatusDataBand``.

Fourth, select **Data** of the second row and then go to **Properties -> Design**. Set **(Name)** to ``AllTasksByStatusDataBand``.

## Editing The Print Route

Go to the **ToDo.API -> Controllers -> ToDoController.cs** file.

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

After that, fetch the statuses and return the Statuses and ToDos in the PDF file.

``` cs hl_lines="47 53-54"
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
            
            var statuses = allTasks.Select(x => x.Status).Distinct().Select(x=> new { Name = x }).ToList();

            return await new FastReportBuilder()
                .AddFastReportFile("Reports/Task.frx")
                .AddDataObject("Task", data)
                .AddDataList("AllTasks", "AllTasksDataBand", allTasks.ToList<object>())
                .AddDataList("Statuses", "StatusDataBand", statuses.ToList<object>())
                .AddDataList("AllTasks", "AllTasksByStatusDataBand", allTasks.ToList<object>(), 3, "[Statuses.Name] == [AllTasks.Status]")
                .GetPDFFile();
        }
    }
}
```