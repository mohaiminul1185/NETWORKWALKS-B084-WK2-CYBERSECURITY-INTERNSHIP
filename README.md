# NETWORKWALKS-B084-WK2-CYBERSECURITY-INTERNSHIP
# Week 2 Cybersecurity Practical — Footprinting & Network Scanning

> **Learning project | Cybersecurity & Ethical Hacking**
>
> This repository documents my Week 2 practical work on footprinting and network scanning. I wrote the notes in my own words to explain what I practiced, which tools I used, and what I observed.

## Overview

During this practical, I explored how security professionals collect publicly available information about a domain and discover devices on a network. I practiced footprinting with Kali Linux tools, used Maltego for domain-based email discovery, and worked with Zenmap to visualize a network scan.

## Objectives

- Understand the purpose of footprinting and reconnaissance.
- Practice using command-line tools in Kali Linux.
- Identify basic domain, DNS, web-server, and website technology information.
- Use Maltego to explore domain-related email information.
- Use Zenmap/Nmap to discover hosts on a local network.
- Record findings and screenshots in a clear, reproducible way.

## Lab Scope & Ethics

- **Footprinting target used in the practical:** `networkwalks.com`
- **Network scanning:** my own/local lab network.
- These activities were performed for educational purposes and within the scope of the assigned lab. Information gathering is not the same as proving a vulnerability.
- Always obtain explicit permission before testing any website, device, or network that you do not own or administer.

---

## Module 1 — Footprinting with Multiple Kali Linux Tools (W2-PM1)

I used several Kali Linux utilities to collect different types of publicly available information about `networkwalks.com`. Each tool answers a different question, so using them together gives a broader picture than relying on one command.

### 1. WHOIS — Domain registration information

```bash
whois networkwalks.com
```

**What I did:** I queried the domain's WHOIS record to review registration and registrar information.

**What I observed in my output:**
- Registrar: GoDaddy.com, LLC
- Domain creation date: 6 November 2019
- Registry expiry date shown: 6 November 2027
- Name servers: `NS6135.HOSTGATOR.COM` and `NS6136.HOSTGATOR.COM`
- DNSSEC status: unsigned
- Registrant details were privacy-protected through Domains By Proxy.

**Learning:** WHOIS can reveal domain lifecycle and DNS-related details, while privacy protection may hide the registrant's personal information.

### 2. WhatWeb — Website technology fingerprinting

```bash
whatweb networkwalks.com
```

**What I did:** I used WhatWeb to identify technologies and web-server information exposed by the website.

**What I observed in my output:**
- Web server: Apache
- CMS: WordPress
- jQuery: 3.7.1
- WordPress Download Manager was detected
- The site redirected from HTTP to HTTPS and returned `200 OK` over HTTPS.

**Learning:** Technology fingerprinting helps understand a website's visible technology stack. A detected version or component is an observation—not automatically a vulnerability.

### 3. Nslookup — DNS lookup

```bash
nslookup networkwalks.com
```

**What I did:** I queried DNS to resolve the domain name to an IP address.

**Result recorded:**

```text
Server:  8.8.8.8
Name:    networkwalks.com
Address: 192.232.216.135
```

**Learning:** DNS resolution connects a domain name with its returned IP address. DNS results may vary depending on resolver, location, or configuration.

### 4. Curl — HTTP response and headers

```bash
curl -i https://networkwalks.com
```

**What I did:** I inspected the HTTP response headers and the beginning of the website's HTML response.

**What I observed in my output:**
- HTTP status: `200`
- Server header: `Apache`
- Content type: `text/html; charset=UTF-8`
- A WordPress REST API link (`/wp-json/`) was present in the response.
- The page title was `Networkwalks Academy`.

**Learning:** HTTP headers and page metadata can provide useful clues about a web application and its configuration.

### 5. WAFW00F — Web Application Firewall identification

```bash
wafw00f https://networkwalks.com
```

**What I did:** I used WAFW00F to check whether a web application firewall could be identified from the site's response.

**Observation recorded in my practical:** ModSecurity (SpiderLabs) was identified.

**Learning:** A WAF can help filter malicious web requests, but identifying a WAF does not confirm that a website is fully secure.

### 6. DNSRecon — DNS record enumeration

```bash
dnsrecon -d networkwalks.com
```

**What I did:** I used DNSRecon to enumerate DNS information for the domain.

**Purpose:** The tool can help examine DNS records such as name servers, mail-related records, and other available DNS information. I used the output as an additional source for the footprinting phase.

**Module takeaway:** I learned how WHOIS, WhatWeb, Nslookup, Curl, WAFW00F, and DNSRecon complement one another during basic footprinting.

---

## Module 3 — Footprinting with Maltego (W2-PM3)

### Installation and configuration

I installed **Maltego Desktop 4.14.0** on Windows, registered an account, and completed the initial application configuration.

### Email footprinting

- **Target domain:** `networkwalks.com`
- **Entity:** Domain
- **Transform:** `To Emails @domain [Search Engine]`
- **Result:** `info@networkwalks.com`
- **Returned entities:** 1

**What I did:** I created a Domain entity in Maltego and ran the email-related transform. The transform completed successfully and returned one email entity associated with the domain.

**Learning:** Maltego represents information as entities and relationships. Transforms help explore connections between data points, but results depend on the data sources available to the transform.

---

## Module 4 — Footprinting with theHarvester (W2-PM4)

### Command used

```bash
theHarvester -d microsoft.com -l 1000 -b Baidu
```

**Command breakdown:**
- `-d microsoft.com` — specifies the domain to investigate.
- `-l 1000` — sets the result limit.
- `-b Baidu` — selects Baidu as the search source.

**Purpose:** theHarvester is used in reconnaissance to collect publicly available information such as email addresses, subdomains, hosts, and related data from supported sources.

> **Evidence note:** This command is the one recorded in my submitted practical file. I have not added any result counts or discoveries here that were not documented in that file.

---

## Module 5 — Network Scanning with Zenmap (W2-PM5)

### Local network discovery

I used **Zenmap (the graphical interface for Nmap)** to scan my local lab network and visualize the discovered hosts.

**Workflow:**
1. Identified the local network/subnet for the lab.
2. Entered the authorized local subnet as the scan target.
3. Selected a basic **Ping Scan** profile to discover active hosts.
4. Reviewed the discovered hosts and scan output.
5. Opened the **Topology** view to visualize the host relationships.
6. Saved screenshots as practical evidence.

**What I learned:** Zenmap makes Nmap scanning easier to understand visually. Host discovery can show which devices respond on a network, while the Topology view helps present the scan in a diagram.

**Important:** A host not responding to a ping scan is not necessarily offline; firewalls and network settings may block discovery probes. Host discovery alone does not establish that a device is vulnerable.

---

## Key Learning & Reflection

This practical helped me understand the difference between **footprinting** and **network scanning**:

- Footprinting collects information about a target from available sources.
- Technology fingerprinting identifies visible web technologies.
- DNS tools help understand domain resolution and DNS records.
- Maltego helps organize information into entities and relationships.
- theHarvester supports public-source reconnaissance.
- Zenmap/Nmap helps discover hosts on a network and visualize scan results.
- Screenshots, commands, and observations make a practical report easier to verify and reproduce.

The most important lesson for me is that reconnaissance is an information-gathering phase. Findings should be documented accurately, interpreted carefully, and followed by further testing only when authorization and scope allow it.

## Evidence

Screenshots and practical documents are included in this repository to show the commands, tool interfaces, and outputs captured during the lab.

## Tools Used

| Tool | Purpose |
|---|---|
| Kali Linux | Linux environment for footprinting utilities |
| WHOIS | Domain registration lookup |
| WhatWeb | Website technology fingerprinting |
| Nslookup | DNS resolution |
| Curl | HTTP headers and response inspection |
| WAFW00F | WAF detection |
| DNSRecon | DNS enumeration |
| Maltego Desktop 4.14.0 | Visual link analysis and email footprinting |
| theHarvester | Public-source reconnaissance |
| Zenmap / Nmap | Network host discovery and visualization |

## Disclaimer

This repository is for **education and ethical cybersecurity learning only**. I do not encourage unauthorized scanning, exploitation, or access to systems. Always respect ownership, privacy, and the rules of engagement.

---
SCREENSHOT:







<br>




<img width="487" height="373" alt="image" src="https://github.com/user-attachments/assets/814040ea-80e9-4ee8-986a-fe426d14fda1" />

<br>

<br>




<br>


<br>


<br>












<img width="482" height="250" alt="image" src="https://github.com/user-attachments/assets/b072e2ea-3ee3-4d86-8f57-02d22b9013fe" />

<br>






<br>





<br>


<br>



<br>





<br>









<img width="485" height="367" alt="image" src="https://github.com/user-attachments/assets/00ea99ff-bb45-40f1-8258-7ec22bc15edc" />





<br>







<br>


<br>




<br>



<br>



<img width="482" height="363" alt="image" src="https://github.com/user-attachments/assets/7fe66826-d953-46e9-9331-4818f5915ea8" />



<br>


<br>



<br>


<br>



<br>
















<img width="480" height="439" alt="image" src="https://github.com/user-attachments/assets/2a18679e-0ec6-4992-beb3-b8dc4e0ccc31" />




<br>





<br>




<br>









<br>



<br>










<img width="548" height="509" alt="image" src="https://github.com/user-attachments/assets/6dbcdb47-681e-4faa-8e3a-e58dd929c6c7" />


<br>
<br>


<br>

<br>



























<img width="524" height="466" alt="image" src="https://github.com/user-attachments/assets/a1a10c1c-4431-4993-bbd5-5d48ea1d35da" />


<br>















<br>






<br>

<br>



<br>









<img width="548" height="464" alt="image" src="https://github.com/user-attachments/assets/21ec8c4a-aaf8-4f2d-9b8d-74a6a89469b9" />







<br>














<br>

<br>

<br>

<br>











<img width="588" height="436" alt="image" src="https://github.com/user-attachments/assets/50c06dca-894d-41c6-b44a-93aacd802c1c" />


<br>
<br>

<br>
<br>



<br>



























<img width="496" height="516" alt="image" src="https://github.com/user-attachments/assets/6dd03951-0f81-4ada-961f-5c324ee43655" />

<br>

























- GitHub: https://github.com/mohaiminul1185/NETWORKWALKS-B084-WK2-CYBERSECURITY-INTERNSHIP
  <br>
- LinkedIn: https://lnkd.in/p/gGQZeuGD

---

*Week 2 Practical — Footprinting & Network Scanning*
