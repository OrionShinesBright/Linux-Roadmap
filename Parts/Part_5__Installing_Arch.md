# Installing Arch Linux

## Setting up the Keyboard and Fonts
To set the keyboard layout to English US:
```zsh
loadkeys us
```
To set a bigger font for your screen:
```zsh
setfont ter-v32n
```
> [!TIP]
> You can press `Ctrl+L` to clear your screen at any point

> [!TIP]
> You can press `Ctrl+C` to stop any running process

## Connecting to the Internet
Here we have two options:<br>
1. Ethernet (Easier, Recommended)
2. Wifi (A few more steps required)
### With Ethernet
All Ethernet connections should work without need for any configuration. Just make sure you have actually plugged it into your device.
### With Wifi
Skip this if you connected using Ethernet. This is not as easy as ethernet. We shall need to use a command called `iwctl` which runs the `iwd` utility.<br>
#### Steps:
💠 **1. Start the iwctl program**
```zsh
iwctl
```
You will see something like `[iwd]#` on your screen. This is the iwctl prompt. We will run the next few steps in here. The following cammands will have the `[iwd]#` promt written on the left. You should only execute whatever is to the right of the prompt.<br>
💠 **2. List out all your wifi devices**
```iwd
[iwd]# device list
```
You should see something like this on your screen:
```txt
[OUTPUT]
                                 Devices                                     *
-------------------------------------------------------------------------------
Name          Address                  Powered        Adapter        Mode
-------------------------------------------------------------------------------
wlan0         xx:xx:xx:xx:xx:xx        on             phy0           station 
```
💠 **3. Power-On your adapter (taking phy0 as example) [will show no output]**
```iwd
[iwd]# adapter phy0 set-property Powered on
```
💠 **4. Power-On your device (taking wlan0 as example) [will show no output]**
```iwd
[iwd]# device wlan0 set-property Powered on
```
💠 **5. Scan for nearby Wifi-networks (taking wlan0 as example) [will show no output]**
```iwd
[iwd]# station wlan0 scan
```
💠 **6. Display a List of scanned networks [Should show output]**
```iwd
[iwd]# station wlan0 get-networks
```
You should see something like this on your screen:
```txt
[OUTPUT]
                               Available networks                             *
--------------------------------------------------------------------------------
      Network name                      Security            Signal
--------------------------------------------------------------------------------
      Very_Cool                         psk                 ****    
```
💠 **7. Connect to your network of choice (using Very_Cool as example)**
```iwd
[iwd]# station wlan0 connect Very_Cool
```
You should see something like this on your screen:
```txt
[OUTPUT]                                        
Type the network passphrase for HUAWEI-qPyg psk.                                
Passphrase: 
```
Please enter your password and press Enter.<br>
💠 **8. Exit `iwctl`**
```iwd
[iwd]# exit
```

### Checking your Internet Connection
You can check your connection by running:
```zsh
ping -c 3 archlinux.org
```
if you see an output like:
```txt
[OUTPUT]
PING archlinux.org (2a01:4f9:c010:6b1f::1) 56 data bytes
64 bytes from archlinux.org (2a01:4f9:c010:6b1f::1): icmp_seq=1 ttl=50 time=242 ms
64 bytes from archlinux.org (2a01:4f9:c010:6b1f::1): icmp_seq=2 ttl=50 time=265 ms
64 bytes from archlinux.org (2a01:4f9:c010:6b1f::1): icmp_seq=3 ttl=50 time=184 ms
 
--- archlinux.org ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2004ms
rtt min/avg/max/mdev = 184.104/230.383/264.706/33.974 ms
```
Then you have working internet.
> [!TIP]
> If you connected using wifi, you may also run the following command, and it will tell you whether you are connected or now:
> ```zsh
> iwctl station wlan0 show | grep State
> ```
