# Setting Up OData on the Project

Before we create the repositories, we need to set up OData on our project.

First, we need to create the EDM model inside ```Program.cs```.

``` cs hl_lines="1-8"
static IEdmModel GetEdmModel()
{
    var builder = new ODataConventionModelBuilder();

    builder.EntitySet<ToDoDTO>("ToDo");

    return builder.GetEdmModel();
}

var builder = WebApplication.CreateBuilder(args);

```

Then, we add Json options to our controller inside ```Program.cs```.

``` cs hl_lines="5-9"
// Add services to the container.

builder.Services.AddDbContext<DB>(x => x.UseSqlServer(builder.Configuration.GetConnectionString("SQLServer")));

builder.Services.AddControllers().AddJsonOptions(options =>
{
    options.JsonSerializerOptions.PropertyNamingPolicy = null;
});
```

After that, we will add OData to the controllers.

``` cs hl_lines="9-14"
// Add services to the container.

builder.Services.AddDbContext<DB>(x => x.UseSqlServer(builder.Configuration.GetConnectionString("SQLServer")));

builder.Services.AddControllers().AddJsonOptions(options =>
{
    options.JsonSerializerOptions.PropertyNamingPolicy = null;
})
.AddOData(opt =>
{
    opt.Count().Filter().Expand().Select().OrderBy().SetMaxTop(1000)
    .AddRouteComponents("odata", GetEdmModel());
});
```