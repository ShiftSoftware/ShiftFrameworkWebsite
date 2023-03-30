# Adding Shift Repositories

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

## Adding Repositories To Our API

Inside **Data/Repositories** folder add a class and name it ``ToDoRepository.cs``.

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

In the class, add ``ShiftRepository`` and ``IShiftRepositoryAsync`` to the class like the example below:

``` cs hl_lines="8-9"
using ShiftSoftware.ShiftEntity.Core;
using ToDo.API.Data.Entities;
using ToDo.Shared.DTOs.ToDo;

namespace ToDo.API.Data.Repositories
{
    public class ToDoRepository :
        ShiftRepository<Entities.ToDo>,
        IShiftRepositoryAsync<Entities.ToDo, ToDoListDTO, ToDoDTO>
    {
    }
}
```

After that, write the constructor and implement Shift Interface methods.

``` cs hl_lines="11-13 15-18 20-23 25-28 30-33 35-38 40-43"
using ShiftSoftware.ShiftEntity.Core;
using ToDo.API.Data.Entities;
using ToDo.Shared.DTOs.ToDo;

namespace ToDo.API.Data.Repositories
{
    public class ToDoRepository :
        ShiftRepository<Entities.ToDo>,
        IShiftRepositoryAsync<Entities.ToDo, ToDoListDTO, ToDoDTO>
    {
        public ToDoRepository(DB db) : base(db, db.ToDos)
        {
        }

        public Task<Entities.ToDo> CreateAsync(ToDoDTO dto, long? userId = null)
        {
            throw new NotImplementedException();
        }

        public Task<Entities.ToDo> DeleteAsync(Entities.ToDo entity, long? userId = null)
        {
            throw new NotImplementedException();
        }

        public Task<Entities.ToDo> FindAsync(long id, DateTime? asOf = null, bool ignoreGlobalFilters = false)
        {
            throw new NotImplementedException();
        }

        public IQueryable<ToDoListDTO> OdataList(bool ignoreGlobalFilters = false)
        {
            throw new NotImplementedException();
        }

        public Task<Entities.ToDo> UpdateAsync(Entities.ToDo entity, ToDoDTO dto, long? userId = null)
        {
            throw new NotImplementedException();
        }

        public Task<ToDoDTO> ViewAsync(Entities.ToDo entity)
        {
            throw new NotImplementedException();
        }
    }
}
```

Then, add database property and change the methods to asynchronous.

``` cs hl_lines="12 16 19 24 29 34 39 44"
using Microsoft.EntityFrameworkCore;
using ShiftSoftware.ShiftEntity.Core;
using ToDo.API.Data.Entities;
using ToDo.Shared.DTOs.ToDo;

namespace ToDo.API.Data.Repositories
{
    public class ToDoRepository :
        ShiftRepository<Entities.ToDo>,
        IShiftRepositoryAsync<Entities.ToDo, ToDoListDTO, ToDoDTO>
    {
        private DB db;

        public ToDoRepository(DB db) : base(db, db.ToDos)
        {
            this.db = db;
        }

        public async Task<Entities.ToDo> CreateAsync(ToDoDTO dto, long? userId = null)
        {
            throw new NotImplementedException();
        }

        public async Task<Entities.ToDo> DeleteAsync(Entities.ToDo entity, long? userId = null)
        {
            throw new NotImplementedException();
        }

        public async Task<Entities.ToDo> FindAsync(long id, DateTime? asOf = null, bool ignoreGlobalFilters = false)
        {
            throw new NotImplementedException();
        }

        public async IQueryable<ToDoListDTO> OdataList(bool ignoreGlobalFilters = false)
        {
            throw new NotImplementedException();
        }

        public async Task<Entities.ToDo> UpdateAsync(Entities.ToDo entity, ToDoDTO dto, long? userId = null)
        {
            throw new NotImplementedException();
        }

        public async Task<ToDoDTO> ViewAsync(Entities.ToDo entity)
        {
            throw new NotImplementedException();
        }
    }
}
```

After that, add the following method:

``` cs hl_lines="49-54"
using Microsoft.EntityFrameworkCore;
using ShiftSoftware.ShiftEntity.Core;
using ToDo.API.Data.Entities;
using ToDo.Shared.DTOs.ToDo;

namespace ToDo.API.Data.Repositories
{
    public class ToDoRepository :
        ShiftRepository<Entities.ToDo>,
        IShiftRepositoryAsync<Entities.ToDo, ToDoListDTO, ToDoDTO>
    {
        private DB db;

        public ToDoRepository(DB db) : base(db, db.ToDos)
        {
            this.db = db;
        }

        public async Task<Entities.ToDo> CreateAsync(ToDoDTO dto, long? userId = null)
        {
            throw new NotImplementedException();
        }

        public async Task<Entities.ToDo> DeleteAsync(Entities.ToDo entity, long? userId = null)
        {
            throw new NotImplementedException();
        }

        public async Task<Entities.ToDo> FindAsync(long id, DateTime? asOf = null, bool ignoreGlobalFilters = false)
        {
            throw new NotImplementedException();
        }

        public async IQueryable<ToDoListDTO> OdataList(bool ignoreGlobalFilters = false)
        {
            throw new NotImplementedException();
        }

        public async Task<Entities.ToDo> UpdateAsync(Entities.ToDo entity, ToDoDTO dto, long? userId = null)
        {
            throw new NotImplementedException();
        }

        public async Task<ToDoDTO> ViewAsync(Entities.ToDo entity)
        {
            throw new NotImplementedException();
        }

        public async Task AssignValue(Entities.ToDo entity, ToDoDTO dto)
        {
            entity.Title = dto.Title;
            entity.Description = dto.Description;
            entity.Status = dto.Status;
        }
    }
}
```

Now, start the methods like the example below:

``` cs hl_lines="19-26 28-33 35-38 40-43 45-52 54-57"
using Microsoft.EntityFrameworkCore;
using ShiftSoftware.ShiftEntity.Core;
using ToDo.API.Data.Entities;
using ToDo.Shared.DTOs.ToDo;

namespace ToDo.API.Data.Repositories
{
    public class ToDoRepository :
        ShiftRepository<Entities.ToDo>,
        IShiftRepositoryAsync<Entities.ToDo, ToDoListDTO, ToDoDTO>
    {
        private DB db;

        public ToDoRepository(DB db) : base(db, db.ToDos)
        {
            this.db = db;
        }

        public async Task<Entities.ToDo> CreateAsync(ToDoDTO dto, long? userId = null)
        {
            var entity = new Entities.ToDo().CreateShiftEntity(userId);

            await this.AssignValue(entity, dto);

            return entity;
        }

        public async Task<Entities.ToDo> DeleteAsync(Entities.ToDo entity, long? userId = null)
        {
            entity.DeleteShiftEntity(userId);

            return entity;
        }

        public async Task<Entities.ToDo> FindAsync(long id, DateTime? asOf = null, bool ignoreGlobalFilters = false)
        {
            return await base.FindAsync(id, asOf, ignoreGlobalFilters);
        }

        public IQueryable<ToDoListDTO> OdataList(bool ignoreGlobalFilters = false)
        {
            return this.db.ToDos.Select(x => (ToDoListDTO)x);
        }

        public async Task<Entities.ToDo> UpdateAsync(Entities.ToDo entity, ToDoDTO dto, long? userId = null)
        {
            entity.UpdateShiftEntity(userId);

            await this.AssignValue(entity, dto);

            return entity;
        }

        public async Task<ToDoDTO> ViewAsync(Entities.ToDo entity)
        {
            return entity;
        }

        public async Task AssignValue(Entities.ToDo entity, ToDoDTO dto)
        {
            entity.Title = dto.Title;
            entity.Description = dto.Description;
            entity.Status = dto.Status;
        }
    }
}
```