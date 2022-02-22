---
layout: post
title:  Start/Stop D365 services through PowerShell
categories: [dynamics365, powershell, d365]
---

## Use the following script to start/stop your D365 services.

### To start:
`StartStopD365Services.ps1`

### To Stop:
`StartStopD365Services.ps1 -action stop`

```Powershell
Param(
    [string]$action = "start"
)

$ErrorActionPreference = 'SilentlyContinue'
#$ErrorActionPreference = 'Continue'
$services = @(
    'DynamicsAxBatch', 
    'Microsoft.Dynamics.AX.Framework.Tools.DMF.SSISHelperService.exe', 
    'MR2012ProcessService', 
    'W3SVC', 
    'ReportServer'
    )

foreach ($service in $services)
{
    net $action $service
}
```

