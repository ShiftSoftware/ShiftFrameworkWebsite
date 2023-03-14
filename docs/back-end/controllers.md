# Creating the Controllers

Create a new class inside ```Controllers``` folder and name it ```ToDoController.cs```. Inside the class you need to inherit ```ShiftEntityControllerAsync<ToDoRepo, ToDo, ToDoListDTO, ToDoViewDTO>```.

``` cs hl_lines="3"
[Route("api/[controller]")]
[ApiController]
public class ToDoController : ShiftEntityControllerAsync<ToDoRepo, ToDo, ToDoListDTO, ToDoViewDTO>
{
}
```

Then we need to inject the ```ToDoRepo``` and pass it to the constructor.

``` cs hl_lines="5-7"
[Route("api/[controller]")]
[ApiController]
public class ToDoController : ShiftEntityControllerAsync<ToDoRepo, ToDo, ToDoListDTO, ToDoViewDTO>
{
    public ToDoController(ToDoRepo repository) : base(repository)
    {
    }
}
```