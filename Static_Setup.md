===========================================
## Static IP
===========================================

By default most routers setup dynamic ips which keeps on changing depending on the ip availibility 


### How to make local IP static (Linux)
```bash
sudo nmcli con mod "Wired connection 1" ipv4.addresses 192.168.1.175/24
sudo nmcli con mod "Wired connection 1" ipv4.gateway 192.168.1.1
sudo nmcli con mod "Wired connection 1" ipv4.dns "8.8.8.8 1.1.1.1"
sudo nmcli con mod "Wired connection 1" ipv4.method manual
sudo nmcli con up "Wired connection 1"
```


### How to make local IP static (Windows)

## 🖥️ 1. GUI Method (Easiest Way)

### Steps

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

✅ Done — your system now has a fixed static IP. You can confirm it by running:
```bash
ipconfig
```
