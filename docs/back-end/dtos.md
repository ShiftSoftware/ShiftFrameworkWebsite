# Createing DTOs for Our Models

We will create two DTO files, one for when we list the ToDos and one for viewing the ToDos in detail. Create a folder and name it ```DTOs```. Then, create another folder inside the newly created folder, and name it ```ToDo```. Iniside the ```ToDo``` folder create the following classes.

First DTO class name it ```ToDoListDTO.cs```, it is for listing the ToDos. Once you create the DTO class, you need to inherit ```ShiftEntityListDTO```.

``` cs hl_lines="1"
public class ToDoListDTO : ShiftEntityListDTO
{
    public string Title { get; set; }
    public bool Done { get; set; }
}
```

Second DTO class name it ```ToDoViewDTO.cs```, it is for viewing the ToDo in detail. For this class, you need to inherit ```ShiftEntityDTO```.

``` cs hl_lines="1"
public class ToDoViewDTO : ShiftEntityDTO
{
    public string Title { get; set; }
    public string Description { get; set; }
    public bool Done { get; set; }
}
```

## Casting Data Model to DTO

In order to map the data models to DTO or vice versa, when we are working with the data. We will write two methods for casting the data model to two different DTOs. Go to ```Data/Entities/ToDo.cs``` and add the following methods.

For casting ```ToDo``` to ```ToDoListDTO```:

``` cs hl_lines="7-20"
public class ToDo : ShiftEntity<ToDo>
{
    public string Title { get; set; }
    public string Description { get; set; }
    public bool Done { get; set; }

    public static implicit operator ToDoListDTO(ToDo t)
    {
        return new ToDoListDTO
        {
            Title = t.Title,
            Done = t.Done,
            ID = t.ID,
        };
    }
}
```

For casting ```ToDo``` to ```ToDoViewDTO```:

``` cs hl_lines="22-36"
public class ToDo : ShiftEntity<ToDo>
{
    public string Title { get; set; }
    public string Description { get; set; }
    public bool Done { get; set; }

    public static implicit operator ToDoListDTO(ToDo t)
    {
        return new ToDoListDTO
        {
            Title = t.Title,
            Done = t.Done,
            CreateDate = t.CreateDate,
            CreatedByUserID = t.CreatedByUserID,
            ID = t.ID,
            IsDeleted = t.IsDeleted,
            LastSaveDate = t.LastSaveDate,
            LastSavedByUserID = t.LastSavedByUserID
        };
    }

    public static implicit operator ToDoViewDTO(ToDo t)
    {
        return new ToDoViewDTO
        {
            Title = t.Title,
            Description = t.Description,
            Done = t.Done,
            CreateDate = t.CreateDate,
            CreatedByUserID = t.CreatedByUserID,
            ID = t.ID,
            IsDeleted = t.IsDeleted,
            LastSaveDate = t.LastSaveDate,
            LastSavedByUserID = t.LastSavedByUserID
        };
    }
}
```