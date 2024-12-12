---
layout: page
title: "Modules and Aliases"
permalink: /KB_Powershell/useful_cmdlets/
---
# PowerShell Useful Cmdlets
### Working with modules 
```powershell
# Import the members of a module into the current session
Import-Module -Name PSDiagnostics

# Import all modules specified by the module path
Get-Module -ListAvailable | Import-Module

# Import the members of several modules into the current session
$module = Get-Module -ListAvailable PSDiagnostics, Dism
Import-Module -ModuleInfo $module

# Restrict module members imported into a session
Import-Module PSDiagnostics -Function Disable-PSTrace, Enable-PSTrace
(Get-Module PSDiagnostics).ExportedCommands

# Import the members of a module and add a prefix
Import-Module PSDiagnostics -Prefix x -PassThru

# Import a module from a remote computer
$session = New-PSSession -ComputerName Server01
Get-Module -PSSession $session -ListAvailable -Name NetSecurity


# Explicit Loading
Import-Module -Name 'AzureAD' -UseWindowsPowerShell

# Implicit Loading
Import-Module -Name 'ServerManager'
Get-Module -Name 'ServerManager'
```
### Exploring Help
```powershell
# Display basic help information about a cmdlet
Get-Help Format-Table
Get-Help -Name Format-Table
Format-Table -?

# Display basic information one page at a time
help Format-Table
man Format-Table
Get-Help Format-Table | Out-Host -Paging

# Display more information for a cmdlet
Get-Help Format-Table -Detailed
Get-Help Format-Table -Full

# Display selected parts of a cmdlet by using parameters
Get-Help Format-Table -Examples
Get-Help Format-Table -Online
Get-Help Format-Table -Parameter *
Get-Help Get-ChildItem -Parameter *
Get-Help Format-Table -Parameter GroupBy

# Display a list of conceptual articles
Get-Help about_*

# Display a list of articles that include a word
Get-Help -Name remoting

# Save the help for specified module
$modulename = ""
$module = Invoke-Command -ComputerName RemoteServer -ScriptBlock { Get-Module -Name $modulename -ListAvailable }
Save-Help -Module $module -DestinationPath "C:\SavedHelp"
```
### Understand Syntax
```powershell
# Creating PowerShell Variables
$variable = "My Text Value"
$variable = Get-ComputerInfo

# PowerShell Pipes
Get-Service | Sort-Object -property Status
"I can now use PowerShell Pipe Commands!!" | Out-File "C:\Training\file.txt"
Get-Service | WHERE {$_.status -eq "Running"} | SELECT displayname

# System Variables
Get-ChildItem Env: #Returns all environment variables
$env:SystemRoot #Returns System Path e.g. C:\Windows
$env:VARIABLE = "My Text Value" #Creates and Sets a new Environment Variable

# Ensuring the right data types for variables
[int32]$var #Displays single number e.g. 1
[float]$var #Displays number with decimal e.g. 1.2
[string]$var #Displays text value e.g. 1.2
[boolean]$var #Displays either true or false e.g. True
[datetime]$var #Displays a date e.g. "Thursday, January 2, 2020 12:00:00 AM" 

Terse Commands

# View details of the Command
Get-Command Format-Table
Get-Command ft

Get-Command
gcm

# View list of Aliases
Get-Alias

# Define custom Aliases
Set-Alias -Name "" -Value "" -Description ""

# Compare
Get-Command | Where-Object {$_.parametersets.count -gt 3} |  Format-List Name
gcm | ? {$_.parametersets.Count -gt 3}| fl Name

Write-Output "Test Message"
echo "Test Message"

Get-Process
gps
```
### Utilize Variables
```powershell
# View Members for an object
Get-Service -ServiceName 'Dnscache' | Get-Member

# Get the type of object
Get-Service | Get-Member -MemberType Property

# Get the properties of the object
Get-Service -ServiceName 'Dnscache' | Select-Object -Property 'StartType'

# Retrieve alias value
Get-Service | Get-Member -MemberType 'AliasProperty'

# Populate variable with object
$svc = Get-Service -ServiceName 'Dnscache'
$svc.Name
$svc.RequiredServices

# View the methods for an object
Get-Service | Get-Member -MemberType 'Method'

# Selecting values from a PowerShell Object
Get-Service -ServiceName * | Select-Object -Property 'Status','DisplayName'

# Sorting values from Object
Get-Service -ServiceName * | Select-Object -Property 'Status','DisplayName' |
    Sort-Object -Property 'Status' -Descending
   
# Filtering the objects
Get-Service * | Select-Object -Property 'Status','DisplayName' |
Where-Object -FilterScript {$_.Status -eq 'Running' -and $_.DisplayName -like "Windows*" |
    Sort-Object -Property 'DisplayName' -Descending | Format-Table -AutoSize
```
### Understand Objects
```powershell
# View Members for an object
Get-Service
Get-Service -ServiceName 'Dnscache' | Get-Member

# Get the type of object
Get-Service | Get-Member -MemberType Property

# Get the properties of the object
Get-Service -ServiceName 'Dnscache' | Select-Object -Property 'StartType'

# Retrieve alias value
Get-Service | Get-Member -MemberType 'AliasProperty'

# Populate variable with object
$Svc = Get-Service -ServiceName 'Dnscache'
$Svc.Name
$Svc.RequiredServices

# View the methods for an object
Get-Service | Get-Member -MemberType 'Method'

# Selecting values from a PowerShell Object
Get-Service -ServiceName * | Select-Object -Property 'Status','DisplayName'
Get-Service -ServiceName *

# Sorting values from Object
Get-Service -ServiceName * | Select-Object -Property 'Status','DisplayName' |
    Sort-Object -Property 'Status' -Descending
```
### Create First Script
```powershell
# Part 1
$variable = Get-Service -Name 'Dnscache'
Write-Host $variable

# Part 2
$variable = Get-Service -Name 'Dnscache'
Write-Host $variable.DisplayName

# Part 3
$variable = Get-Service -Name 'Dnscache'
Write-Host $variable.Name
Write-Host $variable.DisplayName
Write-Host $variable.Description

# Part 4
$variable = Get-Service -Name $service
Write-Host $variable.Name -ForegroundColor Yellow
Write-Host $variable.DisplayName -ForegroundColor Green
Write-Host $variable.Description -ForegroundColor Blue

# Part 5
pushd "C:\Users\Trainer\Documents\PowerShell\Start"
Set-Location "C:\Users\Trainer\Documents\PowerShell\Start"
ls

# Part 6
$service = Read-Host "Please type the service to view"
$variable = Get-Service -Name $service

Write-Host $variable.Name -ForegroundColor Yellow
Write-Host $variable.DisplayName -ForegroundColor Green
Write-Host $variable.Description -ForegroundColor Blue
```
### Parameters Attributes for Scripts
```powershell
# Create functions
Function Display-Message()
{
      Write-Host "My Message" 
}

Function Display-Message($Text)
{
      Write-Host $Text
}

# Change the function to use arguments
Function Display-Message()
{
      [String]$Value1 = $args[0]
      [String]$Value2 = $args[1]

      Write-Host $Value1 $Value2
}

# Change the function to use parameter
Function Display-Message()
{
      Param(
            [parameter(Mandatory=$true)]
            [String]$Text
      )
      Write-Host $Text
}

Function Display-Message()
{
      Param(
            [parameter(Mandatory=$true)]
            [ValidateSet("Lexus","Porsche","Toyota","Mercedes-Benz","BMW","Honda","Ford","Chevrolet")]
            [String]$Text
      )
      Write-Host "I like to drive a "$Text
}
```
### Selecting Information
```powershell
# Select objects by property
Get-Process | Select-Object -Property ProcessName, Id, WS
Get-Process | Select-Object -Property ProcessName

# Select objects by property and format the results$variable
Get-Process Explorer | Select-Object -Property ProcessName -ExpandProperty Modules | Format-List

# Select processes using the most memory
Get-Process | Sort-Object -Property WS | Select-Object -Last 5

# Select unique characters from an array
"One","Two","Three","One","One","Two" | Select-Object -Unique

# Select all but the first object
"One","Two","Three","One","One","Two" | Select-Object -Skip 1

# Demonstrate the intricacies of the -ExpandProperty parameter
$object = [pscustomobject]@{Name="MyObject";Expand=@("One","Two","Three","Four","Five")}
$object | Select-Object -ExpandProperty Expand -Property Name
$object | Select-Object -ExpandProperty Expand -Property Name | Get-Member
```
### Filtering Specific Data
```powershell
# Basic Filtering (-eq, -ne and like)
Get-Command | Where-Object {$_.CommandType -eq 'cmdlet'}
Get-Command | Where-Object {$_.CommandType -ne 'cmdlet'}
Get-Command | Where-Object {$_.Name -like '*invoke*'}
Get-Command | Where-Object {$_.Name -like '*workflow*'}

# Joining Filtering
Get-Command | Where-Object {($_.Name -like '*invoke*') `
  -and ($_.CommandType -eq 'cmdlet')}
Get-Command | Where-Object {($_.Name -like '*invoke*') `
  -and !($_.CommandType -eq 'cmdlet')}

# Use a parameter and filter
Get-Command -CommandType cmdlet | `
    Where-Object {$_.Name -like '*invoke*'}

Get-Service
Get-Service | `
    Where-Object -Property Status -eq -Value 'running'

Get-ChildItem -Path "C:\Users\Trainer" -Filter *ps1

Get-Command -ParameterName Filter
```
### Control the Flow
```powershell
# Basic If Statement
$value = 10
if($value -lt 9) { Write-Host "Value is less than 9" }
if($value -gt 9) { Write-Host "Value is greater than 9" }
if($value -lt 9) { Write-Host "Value is less than 9" } else { Write-Host "Value is greater than 9" }

# Existing If Statements
if () {
    $messageOne = "Matched: This is message one"
} else {
    $messageOne = "Not Matched: This is message one"
}
 
if () {
    $messageTwo = "Matched: This is message two"
} else {
    $messageTwo = "Not Matched: This is message two"
}
 
if () {
    $messageThree = "Matched: This is message three"
} else {
    $messageThree = "Not Matched: This is message three"
}

Write-Host $messageOne
Write-Host $messageTwo
Write-Host $messageThree

[PSCustomObject]@{
    "messageOne" = $messageOne
    "messageTwo" = $messageTwo
    "messageThree" = $messageThree
}

# Ternary Operator
[PSCustomObject]@{
    "messageOne" = (() ? "Matched: This is message one" : "Not Matched: This is message one")
    "messageTwo" = (() ? "Matched: This is message two" : "Not Matched: This is message two")
    "messageThree" = (() ? "Matched: This is message three" : "Not Matched: This is message three")
}

# Switch Statement
$value = Read-Host "Type your favorite car brand"
Switch ($value)
{
    Brand1 {'You typed: Brand 1'}
    Brand2 {'You typed: Brand 2'}
    Brand3 {'You typed: Brand 3'}
    Brand4 {'You typed: Brand 4'}
}

# Switch Statement with Default
$value = Read-Host "Type your favorite car brand"
Switch ($value)
{
    Brand1 {'You typed: Brand 1'}
    Brand2 {'You typed: Brand 2'}
    Brand3 {'You typed: Brand 3'}
    Brand4 {'You typed: Brand 4'}
    default {'You did not type any brand'}
}

# Multiple Switch Statement with Default
$brand1 = Read-Host "Type your first favorite car brand"
$brand2 = Read-Host "Type your second favorite car brand"
Switch ($brand1, $brand2)
{
    Brand1 {'You typed: Brand 1'}
    Brand2 {'You typed: Brand 2'}
    Brand3 {'You typed: Brand 3'}
    Brand4 {'You typed: Brand 4'}
    default {'You did not type any brand'}
}
```
### Define Custom Help
```powershell
# Basic Function
function Add-FourNumbers()
{
    param(
        [Int32]$first,
        [Int32]$second,
        [Int32]$third,
        [Int32]$fourth
    )

    $result = $first + $second + $third + $fourth

    Write-Host "$($first) + $($second) + $($third) + $($fourth) = $($result)"
}

Add-FourNumbers -first 1 -second 1 -third 1 -fourth 1


# Basic Function with Parameter Help
function Add-FourNumbers()
{
    param(
        [Int32]
        # Specifies the first number
        $first,
        [Int32]
        # Specifies the second number
        $second,
        [Int32]
        # Specifies the third number
        $third,
        [Int32]
        # Specifies the fourth number
        $fourth
    )

    $result = $first + $second + $third + $fourth

    Write-Host "$($first) + $($second) + $($third) + $($fourth) = $($result)"
}


# Basic Function with Detailed Help
function Add-FourNumbers()
{
    param(
        [Int32]$first,
        [Int32]$second,
        [Int32]$third,
        [Int32]$fourth
    )

    $result = $first + $second + $third + $fourth

    Write-Host "Here is the full sum"
    Write-Host "$($first) + $($second) + $($third) + $($fourth) = $($result)"

    <#
        .SYNOPSIS
        Adds four numbers together and returns the result.

        .DESCRIPTION
        Adds four numbers together and returns the result.
        Takes any four numbers.

        .PARAMETER first
        Specifies the first number

        .PARAMETER second
        Specifies the second number

        .PARAMETER third
        Specifies the third number

        .PARAMETER fourth
        Specifies the fourth number

        .INPUTS
        None

        .OUTPUTS
        System.String

        .EXAMPLE
        C:\PS> Add-FourNumbers -first 1 -second 2 -third 3 -fourth 4
        Here is the full sum
        1 + 2 + 3 + 4 = 10
    #>
}

Get-Help Add-FourNumbers

# Script Level Help
<#
    .SYNOPSIS
    This is a custom script.

    .DESCRIPTION
    This script contains all the functions needed.

    .INPUTS
    None

    .OUTPUTS
    System.String
#>

Get-Help .\Help.ps1
```
### Manage Files and Folders
```powershell
# Set Variable
$Location = "C:\Users\Trainer\PSFolder\"

# C:\Users\Trainer\PSFolder
# C:\Users\Trainer\PSFolder\TextFiles
# C:\Users\Trainer\PSFolder\Users
# C:\Users\Trainer\PSFolder\Data
# C:\Users\Trainer\PSFolder\PSFolderDelete
# C:\Users\Trainer\PSFolder\PSFolderDelete30
# C:\Users\Trainer\PSFolder\PSFolderNew

Get-Command *Item*

# Retrieve items in a folder
Get-ChildItem -Force $Location

# Retrieve items in a folder and all sub-folders
Get-ChildItem -Force $Location -Recurse

# Include specific file formats
Get-ChildItem -Path $Location -Recurse -Include *.xlsx

# Exclude specific formats
Get-ChildItem -Path $Location -Recurse -Exclude *.xlsx 

# Get all item and filter by last write time
Get-ChildItem -Path $Location -Recurse | Where-Object -FilterScript {($_.LastWriteTime -gt '2020-10-22')}

# Create Directory and File
New-Item -Path "$($Location)\PSFolderNew" -ItemType Directory
New-Item -Path "$($Location)\PSFolderNew\PSFile.txt" -ItemType File

# Create Text, CReate File and add Text
$document = 'Lorem ipsum dolor sit amet consectetur adipiscing elit.' | Out-File -FilePath "$($Location)PSFolder\PSDocument.txt" 

# Removing Items in Folder
Remove-Item -Path "$($Location)\PSFolderDelete" -Recurse

# Copy Files
Copy-Item -Path "$($Location)\Users\Users.xlsx" -Destination "$($Location)\Users\UsersCopy.xlsx"

# Rename Files
Rename-Item -Path "$($Location)\Users\UsersCopy.xlsx" -NewName "UsersCopyCopy.xlsx"

# Rename File Extensions
Get-ChildItem "$($Location)\TextFiles\*.txt" | Rename-Item -NewName { $_.name -Replace '\.txt$','.bak' }
```
### Retrieve Data
```powershell
# Set Variable
$Location = "C:\Users\Trainer\PSFolder\Data"

# Get-Content Command
1..100 | ForEach-Object { Add-Content -Path "$($Location)\PSNumbers.txt" -Value "Line $_." }
Get-Content -Path "$($Location)\PSNumbers.txt"

# Parse XML Data
$Path = "$($Location)\MenuData.xml"
$XPath = "/breakfast_menu/food/name"
Select-Xml -Path $Path -XPath $Xpath | Select-Object -ExpandProperty Node

# CSV Data
Get-Process | Export-Csv -Path "$($Location)\Processes.csv"
$processes = Import-Csv -Path "$($Location)\Processes.csv"
$processes | ft

# CSV Data with Delimiter
Get-Process | Export-Csv -Path "$($Location)\Processes.csv" -Delimiter :
$processes = Import-Csv -Path "$($Location)\Processes.csv" -Delimiter :
$processes | ft

# Import CSV and Loop Each Line
Import-Csv "$($Location)\Employees.csv" | ForEach-Object {
    Write-Host "$($_.FirstName), $($_.LastName) born $($_.DateOfBirth)"
}
```
### Work with JSON
```powershell
# Generate JSON
systeminfo /fo CSV | ConvertFrom-Csv | convertto-json | Out-File  "$($Location)\ComputerInfo.json"

# Load JSON
Get-Content -Path "$($Location)\ComputerInfo.json" | ConvertFrom-JSON

# Load JSON into a Grid View
Get-Content -Path "$($Location)\ComputerInfo.json" | ConvertFrom-JSON | Out-GridView

# Populate Variable with JSON and use Values
$jsonObject = Get-Content -Path "$($Location)\ComputerInfo.json" | ConvertFrom-JSON

$jsonObject.'Host Name'
$jsonObject.'Windows Directory'

# Manually Creating JSON
$jsonObject = @{}
$arrayList = New-Object System.Collections.ArrayList

$arrayList.Add(@{"Name"="Reid";"Surname"="Randolph";"Gender"="M";})
$arrayList.Add(@{"Name"="Scott";"Surname"="Best";"Gender"="M";})
$arrayList.Add(@{"Name"="Isabel";"Surname"="Mays";"Gender"="F";})
$arrayList.Add(@{"Name"="Marcia";"Surname"="Clark";"Gender"="F";})

$employees = @{"Employees"=$arrayList;}

$jsonObject.Add("Data",$employees)
$jsonObject | ConvertTo-Json -Depth 10
```
### Enable Remoting
```powershell
# Enable Remoting
Enable-PSRemoting
Enable-PSRemoting -Force

# Connect to a session
Get-PSSessionConfiguration
$session = New-PSSession -ComputerName localhost -ConfigurationName PowerShell.6
$session = New-PSSession -ComputerName localhost -ConfigurationName
Invoke-Command -Session $session -ScriptBlock { $PSVersionTable }

# Start an Interactive Session
Enter-PSSession localhost
Hostname
Get-UICulture
Exit-PSSession
Get-PSSession

# End an Interactive Session
Exit-PSSession

# Run a Remote Command
Invoke-Command -ComputerName localhost -ScriptBlock { Get-ComputerInfo }

# Remove Sessions
$session
Remove-PSSession -Session 5
Remove-PSSession -Session $session
$session
```
### Combine Commands
```powershell
# Pipe
Get-Commend | Where-Object {} | Sort-Object {} | Select-Object

# The && and || Operators
# The && operator executes the command to the right side of the pipe only if the first command was successful.

#  first command succeeds and the second command is executed
Write-Host "Primary Message" && Write-Host "Secondary Message"

# first command fails, the second is not executed
Write-Error "Primary Error" && Write-Host "Secondary Message"

# The || operator executes the command to the right side of the pipe only if the first command was unsuccessful. So it’s the opposite of the previous one.

# first command succeeds, the second command is not executed
Write-Host "Primary Message" || Write-Host "Secondary Message"

# first command fails, so the second command is executed
Write-Error "Primary Error" || Write-Host "Secondary Message"


# Null-coalescing, assignment, and conditional operators
#PowerShell 7 includes Null coalescing operator ??, Null conditional assignment ??=, and Null conditional member access operators ?. and ?[]

$variable = $null

if ($null -eq $variable) {
      "No Value is Found"
}

$variable = "test"
if($null -eq $variable)
{
      "No Value is Found"
}

if($null -eq $variable) { "Np Value is Found" } else { $variable }


$variable = $null
$variable ?? "No Value is Found"

$variable = "test"
$variable ?? "No Value is Found"
```
### Practical PS Remoting
```powershell
# Command to Execute
$Location = "C:\Users\Trainer\Documents\PowerShell\Start"

$session = New-PSSession -ComputerName localhost -ConfigurationName
Invoke-Command -Session $session -ScriptBlock { "$($Location)\Remote.ps1" }
```
### Remote
```powershell
# PowerShell Script to Retrieve Details about the Desktop
$Location = "C:\Users\Trainer\PSFolder"

# Retrieve Desktop Settings
$desktop = Get-CimInstance -ClassName Win32_Desktop

# Retrieve BIOS Information
$bios = Get-CimInstance -ClassName Win32_BIOS

# Retrieve Processor Information
$processor = Get-CimInstance -ClassName Win32_ComputerSystem | Select-Object -Property SystemType

# Get Computer Manufacturer Details
$manufacturer = Get-CimInstance -ClassName Win32_ComputerSystem

# Get Installed Hotfixes
$hotfixes = Get-CimInstance -ClassName Win32_QuickFixEngineering

# Get Operating System Version Information
$operatingsystem = Get-CimInstance -ClassName Win32_OperatingSystem | `
    Select-Object -Property BuildNumber,BuildType,OSType,ServicePackMajorVersion,ServicePackMinorVersion

# Get Users and Owners
$usergroups = Get-CimInstance -ClassName Win32_OperatingSystem | `
    Select-Object -Property NumberOfLicensedUsers,NumberOfUsers,RegisteredUser

# Get Currently Logged-on User
$loggedon = Get-CimInstance -ClassName Win32_ComputerSystem -Property UserName

# Get All Services Status
$services = Get-CimInstance -ClassName Win32_Service | Select-Object -Property Status,Name,DisplayName

# Create File
$report = "$($Location)\Report.log"
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
```
- [Back to KB for PowerShell Contents](https://dzmitry-h.github.io/personalbrand/KB_Powershell/)
- [Back to Home](https://dzmitry-h.github.io/personalbrand/)
