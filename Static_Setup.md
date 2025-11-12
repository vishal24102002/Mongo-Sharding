===========================================
# Static IP
===========================================

By default, most routers assign **dynamic IPs** (DHCP) that can change each time you reconnect. To make your system always use the same IP, follow the steps below.

## For Linux
===========================================
```bash
ps -e | grep -E "NetworkManager|netplan|ifup"
```
### if the above query shows NetworkManager then use the below code 
```bash
nmcli con show
```

```bash
sudo nmcli con mod "Wired connection 1" ipv4.addresses 192.168.1.175/24
sudo nmcli con mod "Wired connection 1" ipv4.gateway 192.168.1.1
sudo nmcli con mod "Wired connection 1" ipv4.dns "8.8.8.8 1.1.1.1"
sudo nmcli con mod "Wired connection 1" ipv4.method manual
sudo nmcli con up "Wired connection 1"
```

### If your system uses Netplan, edit the config file (the name varies, e.g. /etc/netplan/01-netcfg.yaml):
```bash
sudo nano /etc/netplan/01-netcfg.yaml
```

Example configuration:
```bash
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    enp3s0:
      dhcp4: no
      addresses:
        - 192.168.1.174/24
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]
```

Then apply:
```bash
sudo netplan apply
```

### If your system still uses the interfaces file (older systems):
```bash
sudo nano /etc/network/interfaces
```

Add:
```bash
auto enp3s0
iface enp3s0 inet static
    address 192.168.1.174
    netmask 255.255.255.0
    gateway 192.168.1.1
    dns-nameservers 8.8.8.8 1.1.1.1
```

Then restart networking:
```bash
sudo systemctl restart networking
```

## For Windows
===========================================

### 1. GUI Method (Easiest Way)

#### Steps

1. **Open Network Settings**
   - Press `Windows + R`, type `ncpa.cpl`, and press **Enter** to open *Network Connections*.

2. **Find Your Network Adapter**
   - Right-click your active connection (e.g., *Ethernet* or *Wi-Fi*).
   - Choose **Properties**.

3. **Open IPv4 Settings**
   - In the list, double-click **Internet Protocol Version 4 (TCP/IPv4)**.

4. **Set Your Static IP Manually**
   - Select **Use the following IP address** and fill in values like:
     ```
     IP address:      192.168.1.175
     Subnet mask:     255.255.255.0
     Default gateway: 192.168.1.1
     ```
   - Then choose **Use the following DNS server addresses**:
     ```
     Preferred DNS:  8.8.8.8
     Alternate DNS:  1.1.1.1
     ```

5. **Click OK → Close → Reconnect**

your system now has a fixed static IP. You can confirm it by running:

```bash
ipconfig
```

### 2. CMD Method

To get connected network names 
```bash
netsh interface show interface
```

Set static IP:
```
netsh interface ip set address name="Ethernet" static 192.168.1.175 255.255.255.0 192.168.1.1
```

Set DNS:
```
netsh interface ip set dns name="Ethernet" static 8.8.8.8
netsh interface ip add dns name="Ethernet" 1.1.1.1 index=2
```

Switch back to DHCP (dynamic IP):
```
netsh interface ip set address name="Ethernet" dhcp
netsh interface ip set dns name="Ethernet" dhcp
```

If you’re using Wi-Fi, replace "Ethernet" with "Wi-Fi" or the actual adapter name. You can check it using:
