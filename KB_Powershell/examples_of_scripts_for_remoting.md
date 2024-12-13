# Examples of scripts for remoting
```powershell
Enter-PSSession -ComputerName dzmitryhud002
Get-Service
Exit-PSSession

$session = New-PSSession -ComputerName dzmitryhud002
Enter-PSSession -Session $session
New-Item -Path "C:\NewFile.txt" -ItemType File 
Exit-PSSession
Remove-PSSession -Session $session

Invoke-Command -ComputerName dzmitryhud002 -ScriptBlock {
    New-Item -Path "C:\NewFile.txt" -ItemType File
}
Get-PSSessionConfiguration
$session = New-PSSession -ComputerName dzmitryhud002 
Invoke-Command -Session $session -ScriptBlock { Get-Service }
Enter-PSSession dzmitryhud002
Hostname
Get-UICulture
Exit-PSSession
Get-PSSession
Invoke-Command -ComputerName dzmitryhud002 -ScriptBlock { Get-ComputerInfo }
Remove-PSSession -Session $session
$session
Get-Service | Where-Object {$_.Status -eq 'Running'} | Sort-Object Name | Select-Object Name, Status

$name = $null
$username = $name ?? "Anonymous"
Write-Output $username

$username = "Ronald McDonald" ?? "Anonymous"
Write-Output $username

$username = $null ?? "Anonymous"
Write-Output $username

Invoke-Command -Session $session -ScriptBlock {$RemoteScript = Get-Content -Path "C:\Users\dzmitryhud\Documents\Report1.log"
Write-Output $RemoteScript
}
$GetScript = Get-Content -Path "C:\Users\dzmitryhud\Documents\VisualStudio\Remote.ps1" -Raw
$ForRemote = [ScriptBlock]::Create($GetScript)
$session = New-PSSession -ComputerName dzmitryhud002
Invoke-Command -Session $session -ScriptBlock $ForRemote

Invoke-Command -Session $session -ScriptBlock {$RemoteScript = Get-Content -Path "C:\Users\dzmitryhud\Documents\Report2.log"
Write-Output $RemoteScript}

Invoke-Command -Session $session -ScriptBlock {Get-ChildItem -Path "C:\Users\dzmitryhud\Documents"}
```
