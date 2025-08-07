Perfect. Here’s the **updated `kali-ubuntu-network-setup.md`** file using **your specific IP addresses**:

---

### 📄 `setup/kali-ubuntu-network-setup.md`

````markdown
# Kali - Ubuntu Host-Only Network Setup Guide

This guide explains how to configure a **host-only network** between 3 virtual machines using VMware or VirtualBox for ethical hacking practice.

---

## 🧱 Setup Overview

| Role          | OS          | IP Address       | Purpose                     |
|---------------|-------------|------------------|-----------------------------|
| Kali-Attacker | Kali Linux  | 192.169.200.3    | Attacker machine            |
| Ubuntu-Server | Ubuntu      | 192.169.200.4    | DVWA web application host   |
| Ubuntu-Victim | Ubuntu      | 192.169.200.5    | Victim machine (browser)    |

---

## 🔌 Virtual Network Configuration

### 1. **Create a Host-Only Network**

- Open **VMware** → `Edit` → `Virtual Network Editor`
- Create or select a `Host-only` network (e.g., `VMnet1`)
- **Disable DHCP** — we will assign static IPs manually

---

### 2. **Assign Network Adapter to Each VM**

- For each VM:
  - Go to **VM Settings** → **Network Adapter**
  - Select `Custom: Specific virtual network`
  - Choose `VMnet1` (or the name of your host-only network)

---

## 🌐 Assign Static IPs

### On **Ubuntu-Server** and **Ubuntu-Victim**:

Edit netplan config:
```bash
sudo nano /etc/netplan/01-netcfg.yaml
````

#### Ubuntu-Server (`192.169.200.4`)

```yaml
network:
  version: 2
  ethernets:
    ens33:
      dhcp4: no
      addresses: [192.169.200.4/24]
      gateway4: 192.169.200.1
```

#### Ubuntu-Victim (`192.169.200.5`)

```yaml
network:
  version: 2
  ethernets:
    ens33:
      dhcp4: no
      addresses: [192.169.200.5/24]
      gateway4: 192.169.200.1
```

Apply the changes:

```bash
sudo netplan apply
```

---

### On **Kali-Attacker** (`192.169.200.3`):

Edit the interfaces file:

```bash
sudo nano /etc/network/interfaces
```

Add:

```bash
auto eth0
iface eth0 inet static
  address 192.169.200.3
  netmask 255.255.255.0
  gateway 192.169.200.1
```

Restart networking:

```bash
sudo systemctl restart networking
```

---

## 🔍 Verify Network Connectivity

From Kali:

```bash
ping 192.169.200.4   # Ubuntu-Server
ping 192.169.200.5   # Ubuntu-Victim
```

All machines should be able to ping each other within the host-only network.

---

## 🌐 DVWA Access (Optional)

If **DVWA** is hosted on **Ubuntu-Server**, access it from Kali or the victim’s browser:

```
http://192.169.200.4/dvwa
```

Ensure Apache and DVWA are properly set up and running on the server.

---

## ✅ Setup Complete

You now have an isolated, safe environment for testing:

* ARP Spoofing
* MITM Attacks
* Cookie stealing
* and more…

> ⚠️ For ethical use only. Never run these attacks on real or public networks.


