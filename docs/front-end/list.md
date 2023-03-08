# Creating a List to Show Our To Dos

Inside ``Pages`` folder, create a new Razor component and name it ``ToDoList.razor``. The first step is to set a route to this component, like the following example:

``` razor
@attribute [Route($"/{nameof(ToDoList)}")]
```

After that, we want to show a list of to dos, for that we will use ``ShiftList`` element. Give it two parameters, ``T="@ToDoListDTO"`` and ``Action="/odata/ToDo"``, like the below example:

``` razor hl_lines="3"
@attribute [Route($"/{nameof(ToDoList)}")]

<ShiftList T="@ToDoListDTO" Action="/odata/ToDo"></ShiftList>
```