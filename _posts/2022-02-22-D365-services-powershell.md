---
layout: post
title:  Start/Stop D365 services through PowerShell
categories: [dynamics365, powershell, d365]
---

#### To start:
`StartStopD365Services.ps1`

#### To stop:
`StartStopD365Services.ps1 -action stop`


#### StartStopD365Services.ps1
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

