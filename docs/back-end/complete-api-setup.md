# Complete The API Setup

``` hl_lines="2"
ToDo
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

## Installing The Packages

Go to NuGet Package Manager and install these packages.

1. ``Swashbuckle.AspNetCore``
2. ``Microsoft.AspNetCore.Components.WebAssembly.Server``

## Setting Up Program.cs

Go to **Program.cs**.

``` hl_lines="15"
ToDo
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

First, add the services like the below example:

``` cs hl_lines="5-10"
using Microsoft.EntityFrameworkCore;

var builder = WebApplication.CreateBuilder(args);

builder.Services
    .AddControllers()
    .AddJsonOptions(options =>
    {
        options.JsonSerializerOptions.PropertyNamingPolicy = null;
    });

var app = builder.Build();

app.Run();
```

Second, add the repository.

``` cs hl_lines="13"
using Microsoft.EntityFrameworkCore;
using ToDo.API.Data.Repositories;

var builder = WebApplication.CreateBuilder(args);

builder.Services
    .AddControllers()
    .AddJsonOptions(options =>
    {
        options.JsonSerializerOptions.PropertyNamingPolicy = null;
    });

builder.Services.AddScoped<ToDoRepository>();

var app = builder.Build();

app.Run();
```

After that, add the following codes to **Program.cs**:

``` cs hl_lines="16-23 27-49"
using Microsoft.EntityFrameworkCore;
using ToDo.API.Data.Repositories;
using ShiftSoftware.ShiftEntity.Web.Services;

var builder = WebApplication.CreateBuilder(args);

builder.Services
    .AddControllers()
    .AddJsonOptions(options =>
    {
        options.JsonSerializerOptions.PropertyNamingPolicy = null;
    });

builder.Services.AddScoped<ToDoRepository>();

builder.Services.AddSwaggerGen(c =>
{
    c.DocInclusionPredicate(SwaggerService.DocInclusionPredicate);
});

#if DEBUG
builder.Services.AddRazorPages();
#endif

var app = builder.Build();

#if DEBUG

app.UseBlazorFrameworkFiles();
app.UseStaticFiles();

#endif

app.MapControllers();

app.UseCors(x => x.WithOrigins("*").AllowAnyMethod().AllowAnyHeader());

if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

#if DEBUG

app.MapRazorPages();
app.MapFallbackToFile("index.html");

#endif

app.Run();
```

## Setting Up OData

Inside **Program.cs** add the following code:

``` cs hl_lines="10-17"
using Microsoft.EntityFrameworkCore;
using ToDo.API.Data.Repositories;
using ShiftSoftware.ShiftEntity.Web.Services;
using Microsoft.OData.Edm;
using Microsoft.OData.ModelBuilder;
using Microsoft.AspNetCore.OData;
using ToDo.Shared.DTOs.ToDo;
using ToDo.API.Data;

static IEdmModel GetEdmModel()
{
    var builder = new ODataConventionModelBuilder();

    builder.EntitySet<ToDoListDTO>("ToDo");

    return builder.GetEdmModel();
}

var builder = WebApplication.CreateBuilder(args);

builder.Services
    .AddControllers()
    .AddJsonOptions(options =>
    {
        options.JsonSerializerOptions.PropertyNamingPolicy = null;
    });

builder.Services.AddScoped<ToDoRepository>();

builder.Services.AddSwaggerGen(c =>
{
    c.DocInclusionPredicate(SwaggerService.DocInclusionPredicate);
});

#if DEBUG
builder.Services.AddRazorPages();
#endif

var app = builder.Build();

#if DEBUG

app.UseBlazorFrameworkFiles();
app.UseStaticFiles();

#endif

app.MapControllers();

app.UseCors(x => x.WithOrigins("*").AllowAnyMethod().AllowAnyHeader());

if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

#if DEBUG

app.MapRazorPages();
app.MapFallbackToFile("index.html");

#endif

app.Run();
```

Then, add OData to **Program.cs**.

``` cs hl_lines="27-31"
using Microsoft.EntityFrameworkCore;
using ToDo.API.Data.Repositories;
using ShiftSoftware.ShiftEntity.Web.Services;
using Microsoft.OData.Edm;
using Microsoft.OData.ModelBuilder;
using Microsoft.AspNetCore.OData;
using ToDo.Shared.DTOs.ToDo;
using ToDo.API.Data;

static IEdmModel GetEdmModel()
{
    var builder = new ODataConventionModelBuilder();

    builder.EntitySet<ToDoListDTO>("ToDo");

    return builder.GetEdmModel();
}

var builder = WebApplication.CreateBuilder(args);

builder.Services
    .AddControllers()
    .AddJsonOptions(options =>
    {
        options.JsonSerializerOptions.PropertyNamingPolicy = null;
    })
.AddOData(opt =>
{
    opt.Count().Filter().Expand().Select().OrderBy().SetMaxTop(1000)
    .AddRouteComponents("odata", GetEdmModel());
});

builder.Services.AddScoped<ToDoRepository>();

builder.Services.AddSwaggerGen(c =>
{
    c.DocInclusionPredicate(SwaggerService.DocInclusionPredicate);
});

#if DEBUG
builder.Services.AddRazorPages();
#endif

var app = builder.Build();

#if DEBUG

app.UseBlazorFrameworkFiles();
app.UseStaticFiles();

#endif

app.MapControllers();

app.UseCors(x => x.WithOrigins("*").AllowAnyMethod().AllowAnyHeader());

if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

#if DEBUG

app.MapRazorPages();
app.MapFallbackToFile("index.html");

#endif

app.Run();
```

## Setting Up The Database

Add the database to the services.

``` cs hl_lines="22"
using Microsoft.EntityFrameworkCore;
using ToDo.API.Data.Repositories;
using ShiftSoftware.ShiftEntity.Web.Services;
using Microsoft.OData.Edm;
using Microsoft.OData.ModelBuilder;
using Microsoft.AspNetCore.OData;
using ToDo.Shared.DTOs.ToDo;
using ToDo.API.Data;

static IEdmModel GetEdmModel()
{
    var builder = new ODataConventionModelBuilder();

    builder.EntitySet<ToDoListDTO>("ToDo");

    return builder.GetEdmModel();
}

var builder = WebApplication.CreateBuilder(args);

builder.Services
    .AddDbContext<DB>(x => x.UseSqlServer(builder.Configuration.GetConnectionString("SQLServer")))
    .AddControllers()
    .AddJsonOptions(options =>
    {
        options.JsonSerializerOptions.PropertyNamingPolicy = null;
    })
.AddOData(opt =>
{
    opt.Count().Filter().Expand().Select().OrderBy().SetMaxTop(1000)
    .AddRouteComponents("odata", GetEdmModel());
});

builder.Services.AddScoped<ToDoRepository>();

builder.Services.AddSwaggerGen(c =>
{
    c.DocInclusionPredicate(SwaggerService.DocInclusionPredicate);
});

#if DEBUG
builder.Services.AddRazorPages();
#endif

var app = builder.Build();

#if DEBUG

app.UseBlazorFrameworkFiles();
app.UseStaticFiles();

#endif

app.MapControllers();

app.UseCors(x => x.WithOrigins("*").AllowAnyMethod().AllowAnyHeader());

if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

#if DEBUG

app.MapRazorPages();
app.MapFallbackToFile("index.html");

#endif

app.Run();
```

After that, go to **appsettings.Development.json**.

``` hl_lines="15"
ToDo
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
│   │   ├── appsettings.Development.json
│   ├── Program.cs
│
├── ToDo.Shared
│
├── ToDo.Test
│
├── ToDo.Web
```

Add the connection strings like the following example:

``` json hl_lines="2-4"
{
  "ConnectionStrings": {
    "SQLServer": "Server=localhost\\sqlexpress;Initial Catalog=ToDo;Persist Security Info=True;Integrated Security=SSPI;TrustServerCertificate=True;"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  }
}
```

Lastly, open **Package Manager Console** and type the following commands:

1. ```Add-Migration Init```
2. ```Update-Database```

Now, you can run the API and test it using Swagger.