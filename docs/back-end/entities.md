# Using Shift Entities

## Introduction

Shift Entities Helps you create data models and database operations faster, by:

 -  Inheriting the common attributes from the package.
 -  Implementing history table for each model you create.
 -  Using Repositories for each model and Odata, you can create your CRUD operations faster.

## Installation

You can install Shift Entities using .NET CLI tools, with the following command:
``` sh
dotnet add package ShiftSoftware.ShiftEntity --version 1.3.12-alpha
```
Or you can install it using NuGet Package Manager "```ShiftSoftware.ShiftEntity```".

## Creating a Data Model

After installing Shift Entities, Create a new folder name it ```Data``` and inside it create another folder name it ```Entities```. Inside ```Data/Entities``` Create a new class named ```ToDo.cs```.

Inside ```ToDo.cs```, you need to inherit ```ShiftEntity``` in order to inherit the common attributes for this model.

``` cs hl_lines="1"
public class ToDo : ShiftEntity<ToDo>
{
}
```

!!! Info

    The common attributes that will be inherited from the Shift Entities Class are:
    ``` cs
    [Key, DatabaseGenerated(DatabaseGeneratedOption.Identity)]
    public long ID { get; private set; }
    public DateTime CreateDate { get; private set; }
    public DateTime LastSaveDate { get; private set; }
    public long? CreatedByUserID { get; private set; }
    public long? LastSavedByUserID { get; private set; }
    public bool IsDeleted { get; private set; }
    ```

Inside the class add the following attributes:

``` cs hl_lines="3-5"
public class ToDo : ShiftEntity<ToDo>
{
    public string Title { get; set; }
    public string Description { get; set; }
    public bool Done { get; set; }
}
```

Now we have a data model called ```ToDo``` that inherits ```ShiftEntity``` class.

## Creating the Database

### Installing Related Packages

In order to create an SQLite database to store our data, we need to install two packages. You can install the packages using Visual Studio NuGet Package Manager, or using the following commands in the .NET CLI:

 1-  ```Microsoft.EntityFrameworkCore.Sqlite```
``` sh
dotnet add package Microsoft.EntityFrameworkCore.Sqlite --version 7.0.3
```

!!! Note
    You can use any database you like, but for this tutorial I have used SQLite.

 2-  ```Microsoft.EntityFrameworkCore.Tools```
``` sh
dotnet add package Microsoft.EntityFrameworkCore.Tools --version 7.0.3
```

### Creating the Database Class

After creating our data model, we need a database to store our data. First, create a new class inside ```Data``` folder and name it ```DB.cs```. In order to use Shift Entities with your database, you need to inherit ```ShiftDbContext``` to the class.

``` cs hl_lines="1"
public class DB : ShiftDbContext
{
}
```

After that, add the ToDos table.

``` cs hl_lines="3"
public class DB : ShiftDbContext
{
    public DbSet<ToDo> ToDos { get; set; }
}
```

Then, add the constructor.

``` cs hl_lines="5-7"
public class DB : ShiftDbContext
{
    public DbSet<ToDo> ToDos { get; set; }

    public DB(DbContextOptions option) : base(option)
    {
    }
}
```

### Connect the Database with the Application

First, in the ```appsettings.json``` file, add the connection string.

``` json hl_lines="2-4"
{
  "ConnectionStrings": {
    "SQLiteConnection": "Data Source=ToDo.db"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

!!! info
    If you want to change the name of the database, you can simply change ```ToDo.db``` to ```<the name of your choosing>.db```

After that, in ```Program.cs```, add the database context to the services.

``` cs hl_lines="3"
// Add services to the container.

builder.Services.AddDbContext<DB>(x => x.UseSqlite(builder.Configuration.GetConnectionString("SQLiteConnection"))); 
```

### Migrating the Database

In the Package Manager Console, write the following commands to migrate the database:

``` sh
add-migration Initial 
```

``` sh
update-database
```

Now you have connected your database with the application, and store your data.

### Adding History Table for the Data Model (Optional)

With Shift Entities, you can create a history table for the data model you are creating, just by using the ```TemporalShiftEntity``` tag inside your data model.

``` cs hl_lines="1"
[TemporalShiftEntity]
public class ToDo : ShiftEntity<ToDo>
{
    public string Title { get; set; }
    public string Description { get; set; }
    public bool Done { get; set; }
}
```

After adding the tag, you need to migrate the new changes to the database.

``` sh
add-migration TemporalShiftEntity 
```

``` sh
update-database
```