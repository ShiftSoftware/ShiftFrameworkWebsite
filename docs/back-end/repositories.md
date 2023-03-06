# Adding Shift Repositories

## Creating the DTO files

The first step to use Shift Repositories is to create the DTO files for your models. We will create two DTO files, one for when we list the ToDos and one for viewing the ToDos in detail. Create a folder and name it ```DTOs```. Then, create another folder inside the newly created folder, and name it ```ToDo```. Iniside the ```ToDo``` folder create the following classes.

First DTO class name it ```ToDoListDTO.cs```, it is for listing the ToDos. Once you create the DTO class, you need to inherit ```ShiftEntityMixedDTO```.

``` cs hl_lines="1"
public class ToDoListDTO : ShiftEntityMixedDTO
{
    public string Title { get; set; }
    public bool Done { get; set; }
}
```

Second DTO class name it ```ToDoViewDTO.cs```, it is for viewing the ToDo in detail.

``` cs
public class ToDoViewDTO : ShiftEntityMixedDTO
{
    public string Title { get; set; }
    public string Description { get; set; }
    public bool Done { get; set; }
}
```

### Casting Data Model to DTO

In order to map the data models to DTO or vice versa, when we are working with the data while creating the repositories. We will write a method for casting the data model to a DTO. Go to ```Data/Entities/ToDo.cs``` and add the following methods.

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

## Creating Shift Repositories for ToDo Model

Create a folder inside the ```Data``` folder and name it ```Repos```. Inside that folder create a class and name it ```ToDoRepo.cs```.

First, you need to inherit ```ShiftRepository``` as the following.

``` cs hl_lines="1"
public class ToDoRepo : ShiftRepository<ToDo>
{
}
```

Then add a constructor to the class and pass the ```DB``` we created.

``` cs hl_lines="3-8"
public class ToDoRepo : ShiftRepository<ToDo>
{
    private readonly DB db;

    public ToDoRepo(DB db) : base(db, db.ToDos)
    {
        this.db = db;
    }
}
```

After that we need to implement ```IShiftRepositoryAsync<ToDo, ToDoListDTO, ToDoViewDTO>``` interface to the class.

``` cs hl_lines="1"
public class ToDoRepo : ShiftRepository<ToDo>, IShiftRepositoryAsync<ToDo, ToDoListDTO, ToDoViewDTO>
{
    private readonly DB db;

    public ToDoRepo(DB db) : base(db, db.ToDos)
    {
        this.db = db;
    }
}
```

Then implement the interface functions and write the operation of each function, like the example below:

``` cs hl_lines="10-19 21-25 27-30 32-40 42-50 52-55"
public class ToDoRepo : ShiftRepository<ToDo>, IShiftRepositoryAsync<ToDo, ToDoListDTO, ToDoViewDTO>
{
    private readonly DB db;

    public ToDoRepo(DB db) : base(db, db.ToDos)
    {
        this.db = db;
    }

    public async Task<ToDo> CreateAsync(ToDoViewDTO dto, long? userId = null)
    {
        var todo = new ToDo().CreateShiftEntity(userId);

        todo.Title = dto.Title;
        todo.Description = dto.Description;
        todo.Done= dto.Done;

        return todo;
    }

    public async Task<ToDo> DeleteAsync(ToDo entity, long? userId = null)
    {
        entity.DeleteShiftEntity(userId);
        return entity;
    }

    public async Task<ToDo> FindAsync(long id, DateTime? asOf = null, bool ignoreGlobalFilters = false)
    {
        return await base.FindAsync(id, asOf, ignoreGlobalFilters: ignoreGlobalFilters);
    }

    public IQueryable<ToDoListDTO> OdataList(bool ignoreGlobalFilters = false)
    {
        var todos = db.ToDos.AsNoTracking();

        if (ignoreGlobalFilters)
            todos = todos.IgnoreQueryFilters();

        return todos.Select(x => (ToDoListDTO)x);
    }

    public async Task<ToDo> UpdateAsync(ToDo entity, ToDoViewDTO dto, long? userId = null)
    {
        entity.UpdateShiftEntity(userId);

        entity.Title = dto.Title;
        entity.Done = dto.Done;

        return entity;
    }

    public async Task<ToDoViewDTO> ViewAsync(ToDo entity)
    {
        return entity;
    }
}
```

After creating the ```ToDoRepo.cs```, we need to register it in the services, inside ```Program.cs```.

``` cs hl_lines="5"
// Learn more about configuring Swagger/OpenAPI at https://aka.ms/aspnetcore/swashbuckle
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

builder.Services.AddScoped<ToDoRepo>();
```