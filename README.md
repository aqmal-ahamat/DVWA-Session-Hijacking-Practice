# DVWA Session Hijacking (MITM Cookie Theft)

This project demonstrates a **session hijacking attack** against [DVWA (Damn Vulnerable Web Application)](http://www.dvwa.co.uk/) using a **Man-in-the-Middle (MITM)** approach.

By spoofing ARP traffic and capturing HTTP requests, we extract session cookies from a victim machine and use them to hijack the session.

---

## 💡 Project Summary

- **Victim OS:** Ubuntu (Client)
- **Attacker OS:** Kali Linux
- **Target Web App:** DVWA hosted on Ubuntu (Server)
- **Tools Used:** `arpspoof`, `Wireshark`, `Firefox`

---

## 🧪 Attack Flow

1. Setup 3 VMs on the same virtual network.
2. Victim logs into DVWA on the server.
3. Attacker performs ARP spoofing to become the MITM.
4. Attacker captures packets using Wireshark.
5. Extract session cookie from HTTP request.
6. Inject cookie into Firefox browser and hijack session.

---

## 🧰 Tools & Techniques

- `arpspoof` – For ARP poisoning
- `Wireshark` – To sniff and filter cookies from HTTP
- `Firefox` – To inject session cookie manually

---

## ⚠️ Disclaimer

This project is **strictly for educational use only** in a controlled lab environment.  
Do **not** attempt any of these actions on real systems or without proper authorization.

---

## 📌 References

- [DVWA Official Site](http://www.dvwa.co.uk/)
- [Wireshark Filters](https://wiki.wireshark.org/DisplayFilters)
- [ARP Spoofing](https://linux.die.net/man/8/arpspoof)
- [Session Hijacking Explained](https://owasp.org/www-community/attacks/Session_hijacking)

---









