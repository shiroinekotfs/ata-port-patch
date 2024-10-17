# Part 2 - Patching ATA

> This document is written by @anhvlt-2k6. He writes this document in style, means writing in how he discovered and patch ATA to use another port.
>
> Note that these guides only applies for ATA 1.9 versions

## Recommendations before patching

It is recommended to install [ATA 1.9 Update 3](https://learn.microsoft.com/en-us/advanced-threat-analytics/ata-update-1.9.3-migration-guide) before proceeding. All information about planning, software requirements, upgrade paths, deployments and case usages are available on [Microsoft Learn](https://learn.microsoft.com/en-us/advanced-threat-analytics/what-is-ata).

Also, please stop "Microsoft Advanced Threat Analytics Center" service and backup **EVERYTHING** inside the "Backup" folder, by default it locates in this path.

```cmd
C:\Program Files\Microsoft Advanced Threat Analytics\Center
```

## Finding for target

### 1. Looking for backup files

I first thought that Microsoft ATA is written in C# of .NET 4 family (around 4.5 up to 4.8). However, it is hard to determine which file define port configuration, so I find out just take a look at files in "Backup" folder.
There is an very interesting key in those backup files, under JSON.

```json
...other keys...

"CenterWebApplicationConfiguration": {
    "ServiceListeningIpEndpoint": {
        "_t": "EndpointData",
        "Address": "0.0.0.0",
        "Port": 443
    },
    "CommunicationCookieExpiration": "00:20:00"
},
"CenterWebClientConfiguration": {
    "RetryDelay": "00:00:01",
    "ServiceEndpoints": [
        {
            "_t": "EndpointData",
            "Address": "zone01-NET01.testzone01.asp.theflightsims.eu.org",
            "Port": 443
        }
    ],
    "ServiceCertificateThumbprints": [
        "<value>"
    ]
},

...other keys...
```

Note that both `ServiceListeningIpEndpoint` and `ServiceEndpoints` are configured for port 443, but `ServiceListeningIpEndpoint` is for web configuration, and `ServiceEndpoints` is for client to connect to the host.

I have changed the port for both to "8080", then re-import into the ATA Mongo DB as configuration, where the Mongo DB locates in `C:\Program Files\Microsoft Advanced Threat Analytics\Center\MongoDB\bin`

```powershell
mongoimport.exe --db ATA --collection SystemProfile --file "<SystemProfile.json backup file>" --upsert
```

However, when I restart the service, it seems nothing is running. That leads me to another conclusion, that the port configuration is either in another configuration file, or it is being fixed and cannot be changed by editing existing setting file.

### 2. Reverse engineering ATA

As I look for other files, I see an interesting port configuration in `CenterConfiguration.json`

```json
{
  "DatabaseConfigurationServerEndpoint": {
    "Address": "localhost",
    "Port": 27017
  }
}
```
