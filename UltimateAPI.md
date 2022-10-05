## Serilog

- install
  - `Serilog.AspNetCore`
  - `Serilog.Sinks.File`
  - `Serilog.Sinks.File`
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

