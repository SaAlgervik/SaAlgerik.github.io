---
layout: post
title: Logging
subtitle: Makes All Applications Better
cover-img: https://images.unsplash.com/photo-1454182590747-830146fabd33?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1170&q=80
thumbnail-img: https://images.unsplash.com/photo-1518186285589-2f7649de83e0?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1074&q=80
share-img: /assets/img/path.jpg
tags: [Logging, Serilog]
---

#### Application?
The application has a simple Razor Page with one Get and on Post. On the ***index.html*** we added one button, and when it's clicked on the data from our Cosmos Db will be presented on the page. Below we have a textbox that take in a new user with a submit button and post this to the Cosmos Db. If you would like to test it, you can do that [here](https://webapp20211004130753.azurewebsites.net).

_______________________

This diagram is loaded from Azure.

![Image From Blob](https://blobstoragetestazure.blob.core.windows.net/testblobfromappcad5ecc8-9101-4ab2-bf11-e312fb963029/Nr 2/Untitled Diagram.drawio.png)

________________________
Kusto Queries

![Kusto 1](https://blobstoragetestazure.blob.core.windows.net/testblobfromappcad5ecc8-9101-4ab2-bf11-e312fb963029/Bild1/244181882_924126548205281_8673169159859695174_n.png)
<br>
![Kusto 2](https://blobstoragetestazure.blob.core.windows.net/testblobfromappcad5ecc8-9101-4ab2-bf11-e312fb963029/Bild2/243428300_1057725711710151_4595875578227624427_n.png)

________________________

```
   var configuration = new ConfigurationBuilder() // Gets and builds tge config file from appsettings.json
                .AddJsonFile("appsettings.json")
                .AddEnvironmentVariables()
                .Build();


            Log.Logger = new LoggerConfiguration() // Reads the config and injets Serilog to application
                .ReadFrom
                .Configuration(configuration)
                .CreateLogger();
                 
------------------------------------------------------------------------------   
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "AllowedHosts": "*",
  "ApplicationInsights": {
    "ConnectionString": "InstrumentationKey=edc60599-e76e-40de-9c10-979386c25476;IngestionEndpoint=https://westeurope-5.in.applicationinsights.azure.com/"
  },
  "Serilog": {
    "Using": [
      "Serilog.Sinks.ApplicationInsights"
    ],
    "MinimumLevel": {
      "Default": "Debug",
      "Override": {
        "Microsoft": "Information"
      }
    },
    "WriteTo": [
      {
        "Name": "ApplicationInsights",
        "Args": {
          "instrumentationKey": "97cf960c-aac7-4c1e-a987-3d80f7925bf2",
          "restrictedToMinimumLevel": "Information",
          "telemetryConverter": "Serilog.Sinks.ApplicationInsights.Sinks.ApplicationInsights.TelemetryConverters.TraceTelemetryConverter, Serilog.Sinks.ApplicationInsights"
        }
      }
    ]
  }
}

------------------------------------------------------------------------------   

ItemResponse<User> response = await container.CreateItemAsync<User>(user, new PartitionKey(user.Name)); // Stores response
                TimeSpan elapsedTime = response.Diagnostics.GetClientElapsedTime();  // Gets the Diagnostics time from the response
                if (elapsedTime.TotalSeconds > 2)                                   // If the response time from the database is longer then 2 seconds
                    Log.Warning($"Request time {elapsedTime}");                        //Logs Warning
                else
                    Log.Information($"Request time {elapsedTime}");                 //Else it just Logs informations about the response time.
------------------------------------------------------------------------------   
```

_________________________
### Security
If you have Logging in you application and someone tries to extract infortmation from you, if you have Logging on files you can see what has been taken in a hacking attack. You can se what request that has been done and from where. If someone tries to do alot of requests from in a short time this could be good you know aswell. Maybe someone is trying to hack some account or inject something that can break the code, this would be good to know about right away. 
Good logging will make applications better, easier to fix when they are broken and easier to find problmes due to intrusion.

And if something gets worse over time you can see and compare with updates with what could have caused thinks to get worse. 

_________________________
### Queries
As you see in the images the first one gets all the clients that have an other location the "Vargada" and the second one gets all the one that have severityLevel over 1. The last one gets all the logged errors or risks that is not standard information. Well it's always good to get out the things that has gone wrong.
The other one is to see if new locations that we don'n know about has been using the application.

_________________________
<https://docs.microsoft.com/sv-se/azure/cosmos-db/sql/migrate-dotnet-v3?tabs=dotnet-v3>    
<https://github.com/serilog/serilog-sinks-applicationinsights>  
<https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/>

