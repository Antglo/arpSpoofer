# ARP Spoofing Script

## Overview

This guide explains how to turn your Kali Linux machine into a router using IP forwarding and demonstrates how to use an ARP spoofing script written in Python with `scapy` for testing and educational purposes. This script can manipulate the ARP tables of a victim machine and the router, effectively allowing traffic interception between them (Man-in-the-Middle attack).

### **Disclaimer**: 
This script should only be used for educational purposes and with proper authorization. Unauthorized use of ARP spoofing may be illegal and unethical. Always ensure that you have explicit permission before running any network attack tools.

## Step 1: Enable IP Forwarding on Kali Linux

To route traffic between the victim and the router, you need to enable IP forwarding on your Kali machine. This can be done by writing the value `1` into the `/proc/sys/net/ipv4/ip_forward` file.

### Command to Enable IP Forwarding
```bash
echo 1 > /proc/sys/net/ipv4/ip_forward
```
This command will turn your Kali machine into a router, forwarding packets from one network interface to another.

### To verify that IP forwarding is enabled:
```bash
cat /proc/sys/net/ipv4/ip_forward
```
If the output is `1`, then IP forwarding is enabled.

## Step 2: How the ARP Spoofing Script Works

The script performs ARP spoofing by sending ARP replies to a victim machine and a router, telling each that the Kali machine’s MAC address belongs to the other. This allows the Kali machine to intercept traffic between the victim and the router.

- `arp_spoof`: Sends fake ARP replies to both the victim and the router.
- `arp_restore`: Restores the original ARP tables when the attack is stopped.

The script takes two main arguments:
- `victim_ip`: The IP address of the machine you want to intercept.
- `router_ip`: The IP address of the router (gateway).

### Requirements:
Ensure `scapy` is installed:
```bash
pip install scapy
```

## Step 3: Usage of the Script

### Script Arguments

The script requires two arguments:
1. **Victim IP**: The IP address of the target machine.
2. **Router IP**: The IP address of the router (gateway).

### Example of running the script:

```bash
sudo python3 arp_spoof.py <victim_ip> <router_ip>
```

### Example:
```bash
sudo python3 arp_spoof.py 192.168.1.100 192.168.1.1
```

- This command will initiate the ARP spoofing attack, continuously sending forged ARP packets to the victim and the router.

### Stopping the Script

The attack will run until you manually stop it. To stop the script and restore the original ARP tables, press `Ctrl + C`. The script will automatically restore the ARP tables of both the victim and the router to prevent any network disruption.

## Step 4: Script Explanation

Here is a breakdown of the script components:

1. **Importing Modules**: 
   ```python
   from scapy.all import *
   import sys
   ```

2. **ARP Spoofing Function**:
   - `arp_spoof(dest_ip, dest_mac, source_ip)` sends spoofed ARP replies to either the victim or the router, pretending to be the other.
   
3. **ARP Restoration Function**:
   - `arp_restore(dest_ip, dest_mac, source_ip, source_mac)` sends legitimate ARP replies to restore the ARP tables when the attack is over.

4. **Main Function**:
   - The script gathers the victim and router MAC addresses using `getmacbyip()`.
   - It then enters a loop, continually sending spoofed ARP replies until the user interrupts the script, at which point it restores the ARP tables.

## Conclusion

This script demonstrates how to perform ARP spoofing in a local network environment. Remember to only run this on networks where you have permission, and always restore the ARP tables to their correct state after finishing the test. If you have any questions or run into any issues, feel free to reach out!
