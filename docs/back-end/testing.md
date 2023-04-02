# Testing

``` hl_lines="6"
ToDo
├── ToDo.API
│
├── ToDo.Shared
│
├── ToDo.Test
│   ├── Dependencies
│   ├── UnitTest1.cs
│   ├── Usings.cs
│
├── ToDo.Web
```

## Installing The Packages

Open NuGet Package Manager and install the following two packages:

1. ``Microsoft.AspNetCore.Mvc.Testing``
2. ``Microsoft.EntityFrameworkCore``

## Setting Up The Test Project Files

Create a new folder inside the project and name it ``Tests``. Inside that folder, create a new one and name it ``ToDo``.

``` hl_lines="8-9"
ToDo
├── ToDo.API
│
├── ToDo.Shared
│
├── ToDo.Test
│   ├── Dependencies
│   ├── Tests
│   │   ├── ToDo
│   ├── UnitTest1.cs
│   ├── Usings.cs
│
├── ToDo.Web
```

Inside **Usings.cs** add the following code:

``` cs hl_lines="2-8"
global using Xunit;
global using System.Text.Json;
global using ShiftSoftware.ShiftEntity.Model;
global using System.Text.Json.Nodes;
global using System.Text;
global using Xunit.Abstractions;
global using ToDo.Shared;
global using ToDo.API;
```

Then, go to **ToDo.API** Project.

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
│   │   ├── appsettings.Development.json
│   ├── Program.cs
│
├── ToDo.Shared
│
├── ToDo.Test
│
├── ToDo.Web
```

Add a new class inside the project and name it ``WebMarker.cs``.

``` hl_lines="17"
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
│   ├── WebMarker.cs
│
├── ToDo.Shared
│
├── ToDo.Test
│
├── ToDo.Web
```

After that, go back to **ToDo.Test** project.

``` hl_lines="6"
ToDo
├── ToDo.API
│
├── ToDo.Shared
│
├── ToDo.Test
│   ├── Dependencies
│   ├── Tests
│   │   ├── ToDo
│   ├── UnitTest1.cs
│   ├── Usings.cs
│
├── ToDo.Web
```

Now, we will add three classes inside the project, ``BasicTest.CS``, ``CustomWebApplicationFactory.cs``, ``TestPriorityAttribute.cs``.

``` hl_lines="10-12"
ToDo
├── ToDo.API
│
├── ToDo.Shared
│
├── ToDo.Test
│   ├── Dependencies
│   ├── Tests
│   │   ├── ToDo
│   ├── BasicTest.cs
│   ├── CustomWebApplicationFactory.cs
│   ├── TestPriorityAttribute.cs
│   ├── UnitTest1.cs
│   ├── Usings.cs
│
├── ToDo.Web
```

First, copy the code below and paste the whole code inside **BasicTest.cs**.

``` cs
using ShiftSoftware.ShiftEntity.Model.Dtos;
using ShiftSoftware.ShiftEntity.Model;
using System.Text.Json.Nodes;
using System.Text;
using Xunit.Abstractions;

namespace ToDo.Test;

public class BasicTest<DTO, ListDTO>
    where DTO : ShiftEntityDTO
    where ListDTO : ShiftEntityListDTO
{
    public HttpClient client;
    public ITestOutputHelper output;

    public string ApiItemName { get; set; }
    public string OdataItemName { get; set; }

    public BasicTest(string apiItemName, string odataItemItem, HttpClient client, ITestOutputHelper output)
    {
        this.ApiItemName = apiItemName;
        this.OdataItemName = odataItemItem;

        this.client = client;
        this.output = output;
    }

    public async Task<DTO> Get(long ID, bool ensureSuccessStatusCode = true)
    {
        HttpResponseMessage obj = await client.GetAsync($"/api/{ApiItemName}/{ID}");

        var text = await obj.Content.ReadAsStringAsync();

        if (ensureSuccessStatusCode)
            obj.EnsureSuccessStatusCode();

        var item = JsonNode.Parse(text).Deserialize<ShiftEntityResponse<DTO>>();

        return item!.Entity!;
    }

    public async Task<DTO> Post(DTO dto, bool ensureSuccessStatusCode = true)
    {
        var httpContent = new StringContent(JsonSerializer.Serialize(dto), Encoding.UTF8, "application/json");

        HttpResponseMessage obj = await client.PostAsync($"/api/{ApiItemName}", httpContent);

        var text = await obj.Content.ReadAsStringAsync();

        if (ensureSuccessStatusCode)
            obj.EnsureSuccessStatusCode();

        var item = JsonNode.Parse(text).Deserialize<ShiftEntityResponse<DTO>>();

        return item!.Entity!;
    }

    public async Task<DTO> Put(long ID, DTO dto, bool ensureSuccessStatusCode = true)
    {
        var httpContent = new StringContent(JsonSerializer.Serialize(dto), Encoding.UTF8, "application/json");

        HttpResponseMessage obj = await client.PutAsync($"/api/{ApiItemName}/{ID}", httpContent);

        var text = await obj.Content.ReadAsStringAsync();

        if (ensureSuccessStatusCode)
            obj.EnsureSuccessStatusCode();

        var item = JsonNode.Parse(text).Deserialize<ShiftEntityResponse<DTO>>();

        return item!.Entity!;
    }

    public async Task<DTO> Delete(long ID, bool ensureSuccessStatusCode = true)
    {
        HttpResponseMessage obj = await client.DeleteAsync($"/api/{ApiItemName}/{ID}");

        var text = await obj.Content.ReadAsStringAsync();

        if (ensureSuccessStatusCode)
            obj.EnsureSuccessStatusCode();

        var item = JsonNode.Parse(text).Deserialize<ShiftEntityResponse<DTO>>();

        return item!.Entity!;
    }

    public async Task<List<ListDTO>> OdataList(string? queryString = null, bool ensureSuccessStatusCode = true)
    {
        HttpResponseMessage obj = await client.GetAsync($"/odata/{OdataItemName}{queryString}");

        if (ensureSuccessStatusCode)
            obj.EnsureSuccessStatusCode();

        var text = await obj.Content.ReadAsStringAsync();

        var items = JsonNode.Parse(text)!["value"].Deserialize<List<ListDTO>>();

        return items!;
    }

    public async Task<List<RevisionDTO>> RevisionList(long ID, bool ensureSuccessStatusCode = true)
    {
        HttpResponseMessage obj = await client.GetAsync($"/odata/{OdataItemName}/{ID}/revisions");

        if (ensureSuccessStatusCode)
            obj.EnsureSuccessStatusCode();

        var text = await obj.Content.ReadAsStringAsync();

        var items = JsonNode.Parse(text)!["value"].Deserialize<List<RevisionDTO>>();

        return items!;
    }
}
```

Second, copy the code below and paste the whole code inside **CustomWebApplicationFactory.cs**.

``` cs
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Mvc.Testing;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using Microsoft.IdentityModel.Tokens;
using ShiftSoftware.TypeAuth.Core;
using System.IdentityModel.Tokens.Jwt;
using System.Security.Claims;
using ToDo.API;
using ToDo.API.Data;

namespace ToDo.Test
{
    public class CustomWebApplicationFactory<TStartup> : WebApplicationFactory<TStartup> where TStartup : class
    {
        IConfiguration config;
        static string? token;

        protected override IHost CreateHost(IHostBuilder builder)
        {
            builder.UseEnvironment("Development");

            config = new ConfigurationBuilder()
                   .SetBasePath(AppContext.BaseDirectory)
                   .AddJsonFile("appsettings.json", false, true)
                   .Build();

            var host = builder.Build();

            host.Start();

            var serviceProvider = host.Services;

            using (var scope = serviceProvider.CreateScope())
            {
                var scopedServices = scope.ServiceProvider;
                var db = scopedServices.GetRequiredService<DB>();

                db.Database.EnsureDeleted();
                db.Database.EnsureCreated();
            }

            return host;
        }

        protected override void ConfigureClient(HttpClient client)
        {
            base.ConfigureClient(client);
        }

        protected override void ConfigureWebHost(IWebHostBuilder builder)
        {
            builder
                .ConfigureServices(services =>
                {
                    var descriptor = services.SingleOrDefault(d => d.ServiceType == typeof(DbContextOptions<DB>));

                    if (descriptor != null)
                    {
                        services.Remove(descriptor);
                    }

                    services.AddDbContext<DB>(options =>
                    {
                        options.UseSqlServer(config.GetConnectionString("SQLServer_Test_APIs"));
                    });
                });
        }
    }

    [CollectionDefinition("API Collection")]
    public class DatabaseCollection : ICollectionFixture<CustomWebApplicationFactory<WebMarker>>
    {
        // This class has no code, and is never created. Its purpose is simply
        // to be the place to apply [CollectionDefinition] and all the
        // ICollectionFixture<> interfaces.
    }
}
```

Third, copy the code below and paste the whole code inside **TestPriorityAttribute.cs**.

``` cs
using Xunit.Abstractions;
using Xunit.Sdk;

namespace ToDo.Test
{
    [AttributeUsage(AttributeTargets.Method, AllowMultiple = false)]
    public class TestPriorityAttribute : Attribute
    {
        public int Priority { get; private set; }

        public TestPriorityAttribute(int priority) => Priority = priority;
    }
    public class PriorityOrderer : ITestCaseOrderer
    {
        public IEnumerable<TTestCase> OrderTestCases<TTestCase>(IEnumerable<TTestCase> testCases) where TTestCase : ITestCase
        {
            string assemblyName = typeof(TestPriorityAttribute).AssemblyQualifiedName!;

            var sortedMethods = new SortedDictionary<int, List<TTestCase>>();

            foreach (TTestCase testCase in testCases)
            {
                int priority = testCase.TestMethod.Method
                    .GetCustomAttributes(assemblyName)
                    .FirstOrDefault()
                    ?.GetNamedArgument<int>(nameof(TestPriorityAttribute.Priority)) ?? 0;

                GetOrCreate(sortedMethods, priority).Add(testCase);
            }

            foreach (TTestCase testCase in
                sortedMethods.Keys.SelectMany(
                    priority => sortedMethods[priority].OrderBy(
                        testCase => testCase.TestMethod.Method.Name)))
            {
                yield return testCase;
            }
        }

        private static TValue GetOrCreate<TKey, TValue>(
            IDictionary<TKey, TValue> dictionary, TKey key)
            where TKey : struct
            where TValue : new() =>
            dictionary.TryGetValue(key, out TValue? result)
                ? result
                : (dictionary[key] = new TValue());
    }
}
```

After that, add a new item to the project and name it ``appsettings.json``, by right-clicking on the project and selecting ``Add -> New Item``.

``` hl_lines="10"
ToDo
├── ToDo.API
│
├── ToDo.Shared
│
├── ToDo.Test
│   ├── Dependencies
│   ├── Tests
│   │   ├── ToDo
│   ├── appsettings.json
│   ├── BasicTest.cs
│   ├── CustomWebApplicationFactory.cs
│   ├── TestPriorityAttribute.cs
│   ├── UnitTest1.cs
│   ├── Usings.cs
│
├── ToDo.Web
```

Inside the JSON file, copy the following code and paste it inside the whole file:

``` json
{
  "ConnectionStrings": {
    "SQLServer_Test_APIs": "Server=localhost\\sqlexpress;Initial Catalog=ToDo_Test_APIs;Persist Security Info=True;Integrated Security=SSPI;TrustServerCertificate=True;"
  }
}
```

## Writing The Tests

Add a new class inside **Tests/ToDo** folder and name it ``Basic.cs``.

``` hl_lines="10"
ToDo
├── ToDo.API
│
├── ToDo.Shared
│
├── ToDo.Test
│   ├── Dependencies
│   ├── Tests
│   │   ├── ToDo
│   │   │   ├── Basic.cs
│   ├── appsettings.json
│   ├── BasicTest.cs
│   ├── CustomWebApplicationFactory.cs
│   ├── TestPriorityAttribute.cs
│   ├── UnitTest1.cs
│   ├── Usings.cs
│
├── ToDo.Web
```

First, change the *internal* to *public*.

``` cs hl_lines="3"
namespace ToDo.Test.Tests.ToDo;

public class Basic
{
}
```

Second, Add the **TestCaseOrderer** and **Collection**.

``` cs hl_lines="3-4"
namespace ToDo.Test.Tests.ToDo;

[TestCaseOrderer("ToDo.Test.PriorityOrderer", "ToDo.Test")]
[Collection("API Collection")]
public class Basic
{
}
```

Third, extend **BasicTest** and write the constructor.

``` cs hl_lines="7 9-11"
using ToDo.Shared.DTOs.ToDo;
using ToDo.Shared.Enums;
namespace ToDo.Test.Tests.ToDo;

[TestCaseOrderer("ToDo.Test.PriorityOrderer", "ToDo.Test")]
[Collection("API Collection")]
public class Basic : BasicTest<ToDoDTO, ToDoListDTO>
{
    public Basic(CustomWebApplicationFactory<WebMarker> factory, ITestOutputHelper output) : base("ToDo", "ToDo", factory.CreateClient(), output)
    {
    }
}
```

Fourth, add the following properties:

``` cs hl_lines="9-13"
using ToDo.Shared.DTOs.ToDo;
using ToDo.Shared.Enums;
namespace ToDo.Test.Tests.ToDo;

[TestCaseOrderer("ToDo.Test.PriorityOrderer", "ToDo.Test")]
[Collection("API Collection")]
public class Basic : BasicTest<ToDoDTO, ToDoListDTO>
{
    static long CreatedItemId;

    static string title = "ToDo 1";
    static string description = "ToDo Item 1";
    static ToDoStatus status = ToDoStatus.New;

    public Basic(CustomWebApplicationFactory<WebMarker> factory, ITestOutputHelper output) : base("ToDo", "ToDo", factory.CreateClient(), output)
    {
    }
}
```

Fifth, add the following method:

``` cs hl_lines="19-32"
using ToDo.Shared.DTOs.ToDo;
using ToDo.Shared.Enums;
namespace ToDo.Test.Tests.ToDo;

[TestCaseOrderer("ToDo.Test.PriorityOrderer", "ToDo.Test")]
[Collection("API Collection")]
public class Basic : BasicTest<ToDoDTO, ToDoListDTO>
{
    static long CreatedItemId;

    static string title = "ToDo 1";
    static string description = "ToDo Item 1";
    static ToDoStatus status = ToDoStatus.New;

    public Basic(CustomWebApplicationFactory<WebMarker> factory, ITestOutputHelper output) : base("ToDo", "ToDo", factory.CreateClient(), output)
    {
    }

    public async Task<ToDoDTO> PostOrPut(long? ID, string title, string description, ToDoStatus status)
    {
        var dto = new ToDoDTO
        {
            Title = title,
            Description = description,
            Status = status
        };

        if (ID == null)
            return await base.Post(dto);
        else
            return await base.Put(ID.Value, dto);
    }
}
```

Finally, add these test cases:

``` cs hl_lines="34-51 53-59 61-67 69-75 77-98 100-106 108-114"
using ToDo.Shared.DTOs.ToDo;
using ToDo.Shared.Enums;
namespace ToDo.Test.Tests.ToDo;

[TestCaseOrderer("ToDo.Test.PriorityOrderer", "ToDo.Test")]
[Collection("API Collection")]
public class Basic : BasicTest<ToDoDTO, ToDoListDTO>
{
    static long CreatedItemId;

    static string title = "ToDo 1";
    static string description = "ToDo Item 1";
    static ToDoStatus status = ToDoStatus.New;

    public Basic(CustomWebApplicationFactory<WebMarker> factory, ITestOutputHelper output) : base("ToDo", "ToDo", factory.CreateClient(), output)
    {
    }

    public async Task<ToDoDTO> PostOrPut(long? ID, string title, string description, ToDoStatus status)
    {
        var dto = new ToDoDTO
        {
            Title = title,
            Description = description,
            Status = status
        };

        if (ID == null)
            return await base.Post(dto);
        else
            return await base.Put(ID.Value, dto);
    }

    [Fact(DisplayName = "01. Create"), TestPriority(1)]
    public async Task _01_Create()
    {
        var item = await PostOrPut(
            ID: null,
            title: title,
            description: description,
            status: status
        );

        CreatedItemId = item.ID;

        Assert.Multiple(
            () => Assert.Equal(title, item.Title),
            () => Assert.Equal(description, item.Description),
            () => Assert.Equal(status, item.Status)
        );
    }

    [Fact(DisplayName = "02. List"), TestPriority(2)]
    public async Task _02_List()
    {
        var items = await base.OdataList();

        Assert.Contains(items, x => x.Title == title);
    }

    [Fact(DisplayName = "03. Filter"), TestPriority(3)]
    public async Task _03_Filter()
    {
        var items = await base.OdataList($"?$filter=Title eq 'Random Title'");

        Assert.DoesNotContain(items, x => x.Title == title);
    }

    [Fact(DisplayName = "04. Get"), TestPriority(4)]
    public async Task _04_Get()
    {
        var item = await base.Get(CreatedItemId);

        Assert.Equal(title, item.Title);
    }

    [Fact(DisplayName = "05. Put"), TestPriority(5)]
    public async Task _05_Put()
    {
        var updatedTitle = $"{title} - Updated";
        var updatedDescription = $"{description} - Updated";
        var updatedStatus = ToDoStatus.InProgress;

        var item = await PostOrPut(
            ID: CreatedItemId,
            title: updatedTitle,
            description: updatedDescription,
            status: updatedStatus
        );

        CreatedItemId = item.ID;

        Assert.Multiple(
            () => Assert.Equal(updatedTitle, item.Title),
            () => Assert.Equal(updatedDescription, item.Description),
            () => Assert.Equal(updatedStatus, item.Status)
        );
    }

    [Fact(DisplayName = "07. Get Revisions"), TestPriority(6)]
    public async Task _07_GetRevisions()
    {
        var revisions = await base.RevisionList(CreatedItemId);

        Assert.True(revisions.Count > 0);
    }

    [Fact(DisplayName = "07. Delete"), TestPriority(7)]
    public async Task _07_Delete()
    {
        var item = await base.Delete(CreatedItemId);

        Assert.True(item.IsDeleted);
    }
}
```

Before start running the tests, right-click on appsettings.json and select ``Properties``. Inside Properties, change the value of **Copy to Output Directory** to ``Copy Always``.

Now, you can run the tests by clicking on **Test Explorer**, then clicking on the run button.