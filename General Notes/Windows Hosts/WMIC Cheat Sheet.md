## WMIC Basics

-   `wmic`: Launch the WMIC command prompt
-   `wmic /?`: Display a list of available WMIC commands

## Getting Information

-   `wmic computersystem get name`: Get the name of the computer
-   `wmic csproduct get name, vendor`: Get the name and vendor of the computer's product
-   `wmic os get caption`: Get the name and version of the operating system
-   `wmic bios get serialnumber`: Get the serial number of the BIOS
-   `wmic diskdrive get caption, size`: Get the size of the disk drives
-   `wmic memorychip get capacity`: Get the capacity of the memory chips
-   `wmic processor get name`: Get the name of the processors

## Managing Services

-   `wmic service get name, state`: Get a list of services and their current state
-   `wmic service where name="service name" call startservice`: Start a service
-   `wmic service where name="service name" call stopservice`: Stop a service

## Managing Processes

-   `wmic process get caption, commandline`: Get a list of processes and their command line arguments
-   `wmic process where name="process name" call terminate`: Terminate a process

## Managing Users and Groups

-   `wmic useraccount get name, sid`: Get a list of user accounts and their SIDs
-   `wmic group get name, sid`: Get a list of groups and their SIDs