

# 🌐 IT Incident Report: DNS Misconfiguration – Windows 11

## 📌 Incident Overview
- **Title:** DNS Misconfiguration Causing Internet Issues  
- **Environment:** Windows 11 Pro x64  
- **User Impact:** User can access some websites via IP but cannot resolve domain names (e.g., google.com)

---

## 🧾 Problem Description
User reported that websites were not loading in any browser. Error messages included:

> *“DNS server isn’t responding”*  
> *“Hmm… can’t reach this page”*

However, when testing via IP addresses, some services were accessible. For example:

``powershell
ping 8.8.8.8  → Success  
ping google.com  → Failed (host not found)

## 🔍 Troubleshooting Steps

| Step | Command/Action | Output | Interpretation |
|------|----------------|--------|----------------|
| 1 | `ping 127.0.0.1` | Success | Local TCP/IP stack is working |
| 2 | `ping 8.8.8.8` | Success | Internet connection is active |
| 3 | `ping google.com` | Ping request could not find host | DNS resolution is failing |
| 4 | `ipconfig /all` | DNS server set to `192.168.0.5` | Static DNS server configured |
| 5 | `nslookup google.com` | Timed out or error | DNS server is unreachable or misconfigured |
| 6 | Checked IPv4 settings in Control Panel | DNS set manually to 192.168.0.5 | DNS is not responding or is incorrect |


## 🧩 Root Cause

   The system was using a static DNS IP (192.168.0.5) that was either:

    Not reachable

    Misconfigured

    Not running a DNS service

This prevented name resolution for all domains, despite having working internet access.

## 🛠️ Solution Applied

  1.  Opened Control Panel → Network and Internet → Network Connections

  2. Right-clicked the active network adapter → Properties

  3. Selected Internet Protocol Version 4 (TCP/IPv4) → Properties

  4. Changed DNS settings from manual to automatic (Obtain DNS server address automatically)

  5.  Alternatively, entered public DNS manually:

    Preferred DNS: 8.8.8.8  
    Alternate DNS: 1.1.1.1

   Flushed DNS cache and renewed IP:

    ipconfig /flushdns
    ipconfig /release
    ipconfig /renew

   Retested with:

    ping google.com → Success  
    nslookup google.com → Returns IP address


## ✅  Final Result


    DNS resolution restored

    User can browse the internet normally

    No more errors in browser

    System resolves both internal and external domains


✅ Logged and resolved by: David Vargas
🗓️ Date: 7/17/2025
🖥️ System: Windows 11 Pro
