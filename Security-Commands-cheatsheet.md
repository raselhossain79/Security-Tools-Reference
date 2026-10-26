# Security Commands Cheat Sheet

A quick-reference of 80 practical commands used across a penetration testing engagement, organized by phase — from initial recon to privilege-escalation audits. Pairs with the main [Pentest Arsenal](README.md) tool reference: that file answers "which tool," this one answers "what do I actually type."

> Educational use and authorized security testing only. Always get written permission before scanning, testing, or assessing any system or network you do not own.

---

## Table of Contents

- [01. Recon & OSINT](#01-recon--osint)
- [02. Network Scanning & Enumeration](#02-network-scanning--enumeration)
- [03. Web Application Testing](#03-web-application-testing)
- [04. Vulnerability Assessment](#04-vulnerability-assessment)
- [05. Password & Hash Audit](#05-password--hash-audit)
- [06. Wireless & Packet Analysis](#06-wireless--packet-analysis)
- [07. Forensics & Log Analysis](#07-forensics--log-analysis)
- [08. Linux Privilege Audit](#08-linux-privilege-audit)

---

## 01. Recon & OSINT

| # | Command | Description |
|---|---|---|
| 1 | `whois target.com` | View public domain registration data |
| 2 | `dig target.com any` | Query DNS records |
| 3 | `nslookup target.com` | Check DNS resolution |
| 4 | `amass enum -d target.com` | Discover subdomains |
| 5 | `subfinder -d target.com` | Enumerate public subdomains |
| 6 | `assetfinder --subs-only target.com` | Find related subdomains |
| 7 | `whatweb target.com` | Fingerprint web technologies |
| 8 | `theHarvester -d target.com -b bing` | Gather public emails and hosts |
| 9 | `traceroute target.com` | Map network path |
| 10 | `curl -I https://target.com` | Inspect HTTP headers |

## 02. Network Scanning & Enumeration

| # | Command | Description |
|---|---|---|
| 11 | `nmap -sn 192.168.1.0/24` | Ping sweep a local subnet |
| 12 | `arp -a` | View the ARP cache |
| 13 | `ip neigh` | List neighbor table entries |
| 14 | `netdiscover -r 192.168.1.0/24` | Discover live hosts on a LAN |
| 15 | `masscan 192.168.1.0/24 -p1-1000` | Fast port sweep on a lab subnet |
| 16 | `nmap -sV target.com` | Detect service versions |
| 17 | `nc -zv target.com 22` | Test whether a port is open |
| 18 | `traceroute target.com` | Trace the network route |
| 19 | `tcpdump -i eth0 port 80` | Capture HTTP traffic in a lab |
| 20 | `tshark -i eth0` | Start a live packet capture summary |

## 03. Web Application Testing

| # | Command | Description |
|---|---|---|
| 21 | `nikto -h https://target.com` | Scan a web server for common issues |
| 22 | `gobuster dir -u https://target.com -w wordlist.txt` | Discover hidden directories |
| 23 | `whatweb https://target.com` | Identify web technologies |
| 24 | `curl -I https://target.com` | Inspect HTTP response headers |
| 25 | `wpscan --url https://target.com` | Audit a WordPress site |
| 26 | `feroxbuster -u https://target.com -w wordlist.txt` | Perform recursive content discovery |
| 27 | `nuclei -u https://target.com` | Run template-based checks |
| 28 | `httpx -title -tech-detect -u https://target.com` | Fetch the page title and tech stack |
| 29 | `ffuf -u https://target.com/FUZZ -w wordlist.txt` | Fuzz endpoints and paths |
| 30 | `zap-baseline.py -t https://target.com` | Run a baseline OWASP ZAP scan |

## 04. Vulnerability Assessment

| # | Command | Description |
|---|---|---|
| 31 | `nmap --script vuln target.com` | Run Nmap vulnerability scripts |
| 32 | `nuclei -u https://target.com -severity low,medium,high` | Scan with Nuclei templates |
| 33 | `lynis audit system` | Perform a local security audit |
| 34 | `sslyze target.com` | Analyze TLS configuration |
| 35 | `testssl.sh target.com` | Review SSL/TLS settings |
| 36 | `gvm-start` | Start the GVM/OpenVAS services |
| 37 | `openvas -V` | Check the installed OpenVAS version |
| 38 | `trivy image nginx:latest` | Scan a container image for CVEs |
| 39 | `grype dir:.` | Scan files and dependencies for vulnerabilities |
| 40 | `nikto -h https://target.com` | Audit a web server for known issues |

## 05. Password & Hash Audit

| # | Command | Description |
|---|---|---|
| 41 | `john --wordlist=wordlist.txt hashes.txt` | Audit hashes with John the Ripper |
| 42 | `hashcat -m 1000 hashes.txt wordlist.txt` | Test NTLM hashes in a lab |
| 43 | `hashid hashes.txt` | Identify the hash type |
| 44 | `cewl https://target.com -w wordlist.txt` | Build a custom wordlist |
| 45 | `crunch 8 10 -o wordlist.txt` | Generate candidate passwords |
| 46 | `keepass2john vault.kdbx > kp.hash` | Extract a KeePass hash for audit |
| 47 | `zip2john archive.zip > zip.hash` | Extract a ZIP hash for audit |
| 48 | `pdf2john file.pdf > pdf.hash` | Extract a PDF hash for audit |
| 49 | `samdump2 SYSTEM SAM` | Extract offline Windows hashes |
| 50 | `ssh2john id_rsa > ssh.hash` | Extract an SSH key hash for audit |

## 06. Wireless & Packet Analysis

| # | Command | Description |
|---|---|---|
| 51 | `iwconfig` | View wireless interfaces and settings |
| 52 | `airmon-ng start wlan0` | Enable monitor mode |
| 53 | `airodump-ng wlan0mon` | Capture nearby wireless beacons |
| 54 | `wash -i wlan0mon` | Detect WPS-enabled networks |
| 55 | `kismet` | Launch a wireless monitoring dashboard |
| 56 | `wavemon` | Monitor Wi-Fi signal quality |
| 57 | `tshark -i wlan0` | Inspect wireless packets |
| 58 | `hcxdumptool -i wlan0mon -o capture.pcapng` | Capture lab WPA handshakes |
| 59 | `airdecap-ng -e LabWiFi -p passphrase capture.cap` | Decrypt a lab capture |
| 60 | `tcpdump -i wlan0 port 53` | Monitor DNS traffic on Wi-Fi |

## 07. Forensics & Log Analysis

| # | Command | Description |
|---|---|---|
| 61 | `journalctl -xe` | Review recent system events |
| 62 | `tail -f /var/log/auth.log` | Monitor authentication logs |
| 63 | `lastlog` | Check the most recent logins |
| 64 | `last -a` | Review login history |
| 65 | `grep "Failed password" /var/log/auth.log` | Find failed SSH logins |
| 66 | `ausearch -k login` | Query auditd login events |
| 67 | `volatility -f memory.raw windows.pslist` | List processes from a memory image |
| 68 | `bulk_extractor evidence.img -o output/` | Extract forensic artifacts |
| 69 | `strings suspicious.bin \| head` | Preview printable strings |
| 70 | `sha256sum sample.bin` | Calculate a file hash |

## 08. Linux Privilege Audit

| # | Command | Description |
|---|---|---|
| 71 | `sudo -l` | Review sudo permissions |
| 72 | `find / -perm -4000 -type f 2>/dev/null` | Find SUID binaries |
| 73 | `getcap -r / 2>/dev/null` | List file capabilities |
| 74 | `ps aux --sort=-%mem \| head` | Review memory-heavy processes |
| 75 | `ss -tulpen` | List listening services |
| 76 | `crontab -l` | Inspect current user cron jobs |
| 77 | `systemctl list-unit-files --type=service` | Review installed services |
| 78 | `uname -a` | Check the kernel version |
| 79 | `cat /etc/os-release` | Display OS release details |
| 80 | `lsof -i -P -n` | List open network files |

---

## Notes

- Commands that take `wordlist.txt` assume a wordlist is already present (e.g. SecLists, rockyou.txt, or one built with `cewl`/`crunch` from this sheet).
- `target.com` is a placeholder — replace with an in-scope asset only, and confirm authorization before running anything beyond passive recon (#01).
- `volatility` syntax here follows the common CLI form; check `vol.py`/`vol3` invocation details for your installed version.

## Disclaimer

All commands are intended for **authorized security testing, research, and educational purposes only**. Always obtain explicit written permission before scanning, testing, or assessing any system or network you do not own. Unauthorized access or testing may violate applicable laws.
