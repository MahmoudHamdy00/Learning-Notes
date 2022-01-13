# ASP.NET Core Web API

## Entity Framework

### Add DpContext and models

1. add a model (class with all the table attribute)
2. add DB Context
   - Install-Package add `Microsoft.EntityFrameWorkCore`
   - add AppDbContect inherits from `DbContext`
   - create a ctor with `DbContectOptions<AppDbContext> options` and path it to the base ctor
   - define the table names (`DbSet<ModelName>TableName{get,set};`
   - add connection string in the `appsettings.json` -> `ConnectionStrings":{"default":""}`
     - Install-Package add `Microsoft.EntityFrameWorkCore.SqlServer`
   - in the start app (.Net 5)
     - declare a string to store the conn string and in the ctor initialize it with the value (`Configration.getConnectionString("default")`
     - in the `ConfigureServices` method (`services.ddDbContext<DbNameContect>(options =>options.UseSqlServer(connString));`
   - in the program.cs (.Net 6)
     - declare a string to store the conn string and initialize it with the value (`builder.Configration.getConnectionString("default")` -`builder.services.addDbContext<DbNameContect>(options =>options.UseSqlServer(connString);`
     - `builder.services.adddatabasedeveloperpageexceptionfilter()`

### Relations

- Many To Many
- create a bridge model and set it in the two models
- override onModelCreating that takes modelBuilder
- modelBuilder.Entity<Book_Author>().hasone(b=>b.book).withMany(ba=>ba.book_author).hasForgnkey(bi=>bi.bookid); \* modelBuilder.Entity<Book_Author>().hasone(b=>b.author).withMany(ba=>ba.book_author).hasForgnkey(bi=>bi.authorid);

### Migrations

- Install-Package add `Microsoft.EntityFrameWorkCore.Tools`
- Add-Migration message
- Update-Database
- Seeding
  - create a class (AppDbInitializer)
  - create a function `Seed` that takes IApplicationBuilder as an argument
  - get the context by

  ```C#
  using (var serviceScope = applicationBuilder.ApplicationServices.CreateScope())` {
  var context = serviceScope.ServiceProvider.GetService<AppDbContext>();
    if (!context.Books.Any()) context.Books.add?range?(......)
  ```

## Controllers

- add clase named nameService
- create a ctor that takes appDpContext to initialize the appdpcontext with it
- create a function that you want
- add the service in program.cs (`builder.Services.AddTransient<nameService>();`)
- in the controler
- create instance from the service
- ctor to initialize it
- create the function (above it the http method like [HTTPPOST("custom-name")]

return \_context.Books.Where(book => book.id == id).Select(book => new BookWithAuthorVM()
{
title = book.title,
description = book.description,
isRead = book.isRead,
dateRead = book.dateRead,
rate = book.rate,
publisherName = book.publisher.name,
authorsNames = book.book_Authors.Select(y => y.author.name).ToList(),
}).FirstOrDefault();

## Response code

- 1xx informational
- 2xx Success
  - 200 Ok request has succeeded
  - 201 created , a new resource has been created as a resalt
  - 204 no content -No need to return a response body
- 3xx redirection
- 4xx Client Error
  - 400 Bad request the request coudln't be understood by the server due to incorrect syntax
  - 401 Unauthorized ,the request requires user authintication
  - 403 Forbidden -unauthorized request
  - 404 Not Found -the server cannot find the requested resourse
  - 409 Conflict- the request coudn't be completed due to conflict
- 5xx Server Error
  - 500 internal servel error -unexpected error (exception condtion)

## Middleware

- create errorVM Model
- create a static class exceptionMiddlewareExtentions

-
-
-
-

## Return types

- Specific type
- `IActionResult`
- `IActionResult<T>`
- Custom return type

## Data Sorting, Filtering, and Paging

- Sorting
  - list.orderBy?ByDescending?(n=>n.name).toList
- Filtering
  - list.where(n=>n.name.contains("be7a")).toList();
- Paging

  - add `paginated<T>` class
  - `IQueryable<T>` source
  -`items = source.Skip((pageIndex - 1) _ pageSize).Take(pageSize).ToList();`
  - `return new PaginatedList<T>(items/_, count, pageIndex, pageSize\*/);`

  - `this.AddRange(items);` (in the ctor)

## Web Api Versioning

- Query String
- URL Path
- HTTP Header
- Content/Media Type

- Add versioning package (Microsoft.AspNetCore.Versioning)
- include it in program.cs (services.addApiVersioning(config=>{config.defaultApiVersion=new ApiVersion(1,0); config.AssumeDefaultVersionWhenUnspesified=true;} )
  - Query String
  - in the controler [ApiVersion("1.0")]
  - in the query string ?api-version=1.0

- URL Path
  - [Route("api/v{version:apiVersion}/[controller]")]
- HTTP Header
  - in program.cs (services.addApiVersioning(config=>{config.defaultApiVersion=new ApiVersion(1,0); config.AssumeDefaultVersionWhenUnspesified=true; config.ApiVersionReader=new HeaderApiVersionReader("custom-version-header")} )
  - [HttpGet("getPublisherById/{id}" , MapToApiVersion(2.0))]
- Content/Media Type

  - in program.cs (services.addApiVersioning(config=>{config.defaultApiVersion=new ApiVersion(1,0); config.AssumeDefaultVersionWhenUnspesified=true; config.ApiVersionReader=new MediaTypeApiVersionReader()} )
  - [HttpGet("getPublisherById/{id}" , MapToApiVersion(2.0))]
  - Content-Type:text/plain;v=2.0

- Loging

  - Serilog.AspNetCore, Serilog.Sinks.File and Serilog.Sinks.MSSqlServer
  - Log.Logger = new LoggerConfiguration().CreateLoger() in main function
  - builder.Host.UseSerilog(); in program.cs
  - //builder.Host.UseSerilog((ctx, lc) => lc.WriteTo.File("Logs/log.txt",rollingInterval:RollingInterval.Day));
    \*{ var configuration = new ConfigurationBuilder().AddJsonFile("appsettings.json").Build();builder.Host.UseSerilog((ctx, lc) => lc.ReadFrom.Configuration(configuration));}

  ```json
  "Serilog": {
    "MinimumLevel": {
      "Default": "Information",
      "Override": {
        "System": "Error",
        "Microsoft": "Error"
      }
    },
    "WriteTo": [
      {
        "Name": "File",
        "Args": {
          "path": "Logs/log.txt",
          "rollingInterval": "Day"
        }
      },
    {
        "Name": "MSSqlServer",
        "Args": {
          "connectionString": "Data Source=.;Initial Catalog=BooksDb;Integrated Security=True",
          "tableName": "Logs"
        }
      }
    ]
  }

  ```

- Testing Levels
  - Unit Testing - test a single unit of code
  - Integration testiong - Test multipe units together
  - System Testing - test all components together as a whole
  - Acceptence Testing - test if app complies with end-user requirments
- Unit testing Framework

  - NUnit
  - XUnit.Net
  - MSTest

## NUnit

- install inmemory package
- In unittest.cs

```C#
 private static DbContextOptions<AppDbContext> dbContextOptions = new DbContextOptionsBuilder<AppDbContext>()
          .UseInMemoryDatabase(databaseName: "BookDbTest")
          .Options;
 AppDbContext dbContext;
 [OneTimeSetUp]
      public void Setup()
      {
          dbContext = new AppDbContext(dbContextOptions);
          dbContext.Database.EnsureCreated();
          SeedDatabae();
      }
      [Test]
      public void Test1()
      {
  var resalt=lld
  Assert.That(resalt,Is.Equal());
      }

      [OneTimeTearDown]
      public void CleanUp()
      {
          dbContext.Database.EnsureDeleted();
      }

      private void SeedDatabae()
      {
          //add some data
      }
```
