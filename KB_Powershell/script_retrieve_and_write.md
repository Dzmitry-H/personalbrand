---
layout: default
title: "Example of Script (Retrieve Some Sytem Data and Write It to a File)"
permalink: /KB_Powershell/script_retrieve_and_write/
---
# Example of Script (Retrieve Some Sytem Data and Write It to a File)
$ScriptItself = 

{ __# PowerShell Script to Retrieve Details about the Desktop__

$Location = "C:\Users\dzmitryhud\Documents"

__# Retrieve Desktop Settings__

$desktop = Get-CimInstance -ClassName Win32_Desktop

__# Retrieve BIOS Information__

$bios = Get-CimInstance -ClassName Win32_BIOS

__# Retrieve Processor Information__

$processor = Get-CimInstance -ClassName Win32_ComputerSystem | Select-Object -Property SystemType

__# Get Computer Manufacturer Details__

$manufacturer = Get-CimInstance -ClassName Win32_ComputerSystem

__# Get Installed Hotfixes__

$hotfixes = Get-CimInstance -ClassName Win32_QuickFixEngineering

__# Get Operating System Version Information__

$operatingsystem = Get-CimInstance -ClassName Win32_OperatingSystem | `
    Select-Object -Property BuildNumber,BuildType,OSType,ServicePackMajorVersion,ServicePackMinorVersion

__# Get Users and Owners__

$usergroups = Get-CimInstance -ClassName Win32_OperatingSystem | `
    Select-Object -Property NumberOfLicensedUsers,NumberOfUsers,RegisteredUser

__# Get Currently Logged-on User__

$loggedon = Get-CimInstance -ClassName Win32_ComputerSystem -Property UserName

__# Get All Services Status__

$services = Get-CimInstance -ClassName Win32_Service | Select-Object -Property Status,Name,DisplayName


__# Create File__

_$report_ = "$($Location)\Report.log"

New-Item $report -ItemType File -Value "Desktop Report"

Add-Content $report "********** Desktop Details **********"

Add-Content $report $desktop

Add-Content $report "********** Bios Details **********"

Add-Content $report $bios

Add-Content $report "********** Processor Details **********"

Add-Content $report $processor

Add-Content $report "********** Manufacturer Details **********"

Add-Content $report $manufacturer

Add-Content $report "********** Hotfix Details **********"

Add-Content $report $hotfixes

Add-Content $report "********** Operating System Details **********"

Add-Content $report $operatingsystem

Add-Content $report "********** Users and Groups Details **********"

Add-Content $report $usergroups

Add-Content $report "********** Logged On User Details **********"

Add-Content $report $loggedon

Add-Content $report "********** Services Details **********"

Add-Content $report $services

}

$ForRun = [ScriptBlock]::Create($ScriptItself)

do {

$value = Read-Host "Do you really want to run it (Y/N)?"

switch ($value.ToUpper()) {

'Y' {

& $ForRun
           
$validInput = $true
        
}
        
'N' {

Write-Host 'Ok, not now'

$validInput = $true

}

default {

Write-Host 'Please enter either "Y" or "N"'

$validInput = $false

}

}

} until ($validInput)

- [Back to KB for PowerShell Contents](https://dzmitry-h.github.io/personalbrand/KB_Powershell/kb_for_powershell/)
- [Back to Home](https://dzmitry-h.github.io/personalbrand/)

