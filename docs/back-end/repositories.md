# Adding Shift Repositories

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