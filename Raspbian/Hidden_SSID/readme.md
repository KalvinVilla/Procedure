# Raspbian 11 - How to connect hidden SSID

## Requierement
`apt install wpa_supplicant`

## Link
- [wpa configuration](https://debian-facile.org/doc:reseau:wpasupplicant)
- [wpa hidden configuration](https://gist.github.com/zhanglongqi/f05f569182910c49ae4375ed61ea12f8)
- [network interface configuration](https://raspberrypi.stackexchange.com/questions/67311/failed-to-connect-to-non-global-ctrl-ifname-when-running-wpa-cli-reconfigure)

## Configure wpa supplicant
### In a bash script :
```bash
#!/bin/bash

SSID='<YOURSSID>'
PASSWORD='<YOURPASSWORD>'

wpa_passphrase $SSID $PASSWORD >> /etc/wpa_supplicant/wpa_supplicant.conf
```
<p style="color:red">/!\ Don't forget the quotation mark, especially if your use password with special charactere</p>


### Execute script :
```
chmod +x <yourscript>.sh
./<yourscript>.sh
```

### Edit the wpa config file
***vim /etc/wpa_supplicant/wpa_supplicant.conf***
```
network={
    ssid="<YourSSID>"
    scan_ssid=1 # -- ADD THIS --
    #psk="YOURPASSWORD" -- REMOVE THIS --
    psk="0f5zf06zf6z..." -- AUTO GENERATE DONT TOUCH --
    id_str="<YourNetworkName>" -- Add this --
}
```

## Configure network interface
### Edit the network interface
***vim /etc/network/interfaces***
```
auto <INTERFACE WLAN>
iface <INTERFACE WLAN> inet manual
    wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf

iface <YourNetworkName> inet dhcp
```

### Restart service
```
systemctl restart networking
```
