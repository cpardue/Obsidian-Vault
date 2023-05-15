To get the SSID and passphrase for WiFi networks using WMIC, you can use the following command:

```
wmic path win32_networkadapter where "NetConnectionStatus=2" get Description,ServiceName,Name
```


This command will list the descriptions, service names, and names of all network adapters that are currently connected to a network (`NetConnectionStatus=2`). The `Description` and `Name` properties will typically contain the SSID of the WiFi network, while the `ServiceName` property will contain the name of the service that is responsible for managing the network adapter.

To get the passphrase for a WiFi network, you will need to use a different command. One way to do this is to use the `netsh` command-line utility, which can be used to view and modify the network configuration of a Windows machine. Here is an example of how you can use `netsh` to get the passphrase for a WiFi network:

```
netsh wlan show profiles name="SSID_NAME_HERE" key=clear
```


Replace "SSID_NAME_HERE" with the actual SSID of the WiFi network. This command will display the security settings for the WiFi network, including the passphrase (also known as the "network security key").