# Penetration Testing Technical Report 

## Objective

This Technical Report explains in detail the vulnerabilities Artemis, Inc. faces by a thorough penetration test. It shows in detail the assessment overview. It lists the risk factors in detail, along with their security ratings. The scope of this project is defined and agreed upon by both the pen testers and the executives of the client, Artemis Inc. Technical Findings will be included in detail.

### Skills Learned

Technical Security Skills

-OSINT reconnaissance and passive information gathering
-Network scanning, enumeration, and host discovery
-Vulnerability assessment and CVSS scoring
-Web application testing (SQLi, broken access control, sensitive data exposure)
-Exploit development and validation
-Cloud security assessment (AWS)
-Post-exploitation techniques (lateral movement, privilege escalation, credential harvesting)

Analytical and Reporting Skills

-Correlating findings across multiple tools into cohesive attack narratives
-Assigning accurate risk ratings and CVSS scores
-Modeling realistic attacker behavior and attack chains
-Writing clear technical findings for both technical and executive audiences
-Structuring prioritized remediation guidance

### Tools Used

OSINT and Reconnaissance

Google Dorking, Whois/Domain Tools, Nslookup/dig, crt.sh
Amass, Subfinder, Shodan, Censys, theHarvester
Hunter.io, VoilaNorbert, Maltego, Recon-ng, SpiderFoot
GitHub/GitLab search, FOCA, Wayback Machine
LinkedIn/Twitter/Facebook, ZoomEye, BinaryEdge, Netcraft
Google Maps, Reverse Whois, Passive DNS services

Host Discovery and Network Enumeration

-Nmap — port scanning and service fingerprinting
-Fierce — subdomain and internal IP discovery
-OpenVAS — deep service scanning and vulnerability fingerprinting
-Metasploit Framework — vulnerability validation and banner grabbing
-Nikto — web server misconfiguration and outdated software detection

Vulnerability Scanning

-Tenable Nessus — deep network and host vulnerability analysis
-OpenVAS (Greenbone) — vulnerability management and reporting
-Burp Suite — web application traffic interception and vulnerability scanning
-Wapiti — command-line web application vulnerability scanning
-Nikto — web server security issue detection

Exploitation and Post-Exploitation

-SQLmap — automated SQL injection exploitation
-Hydra / Medusa — online brute-force and credential stuffing
-Mimikatz — credential harvesting from Windows systems
-CrackMapExec — network-wide credential testing and enumeration
-Hashcat / John the Ripper — offline password hash cracking

## Penetration Testing Report

**Penetration Testing Technical Report for Artemis, Inc**

Author: Michael Wickland, [wicklandmichael@gmail.com](mailto:wicklandmichael@gmail.com)

Accepted: 10/10/2025

Abstract

This Technical Report explains in detail the vulnerabilities Artemis, Inc. faces by a thorough penetration test. It shows in detail the assessment overview. It lists the risk factors in detail, along with their security ratings. The scope of this project is defined and agreed upon by both the pen testers and the executives of the client, Artemis Inc. Technical Findings will be included in detail.

**Table of Contents**

1. **Scope of Work**  
   1. Reconnaissance Methods  
   2. Targets Identified and Scans Run  
   3. Vulnerabilities Identified  
   4. Analysis and Cleanup

2. **Project Objectives**

3. **Assumptions & Rules of Engagement**

4. **Timeline**

5. **Summary of Findings**  
   1. Table of Vulnerabilities and CVSS scores  
   2. Detailed Findings  
      1. Unpatched RDP Exposed to the Internet  
      2. Web Application Vulnerable to SQL Injection  
      3. Default Password on Cisco Admin Portal  
      4. Apache Web Server Vulnerable to CVE-2019-0211  
      5. Web Server Exposing Sensitive Data  
      6. Web Application has broken Access Control  
      7. Oracle WebLogic Server Vulnerable to CVE-2020-14882  
      8. Misconfigured Cloud Storage (AWS Security Group / S3 Misconfiguration)  
      9. Microsoft Exchange Server Vulnerable to CVE-2021-26855

6. **Recommendations (Listed under each Vulnerability found)**  
   1. Unpatched RDP Exposed to the Internet  
   2. Web Application Vulnerable to SQL Injection  
   3. Default Password on Cisco Admin Portal  
   4. Apache Web Server Vulnerable to CVE-2019-0211  
   5. Web Server Exposing Sensitive Data  
   6. Web Application has broken Access Control  
   7. Oracle WebLogic Server Vulnerable to CVE-2020-14882  
   8. Misconfigured Cloud Storage (AWS Security Group / S3 Misconfiguration)  
   9. Microsoft Exchange Server Vulnerable to CVE-2021-26855  
        
7. **References**

**Introduction**   
This Technical Report explains in detail how Artemis, INC. was, and was not, prepared for a penetration test. It shows in detail the assessment overview. It lists the risk factors in detail, along with their security ratings. The scope of this project is defined and agreed upon by both the pen testers and the executives of the client, Artemis Inc. Technical Findings will be included in detail most of all.

**1.1 	Scope of Work**

Artemis, Inc. engaged my company to provide penetration testing services to detect any vulnerabilities in the network and in the procedures done at Artemis, like email. This engagement is a simulated external penetration test in the form of a structured walkthrough. Scope elements: 

* Targets: Artemis external internet-facing assets discovered during research phases and in identifying vulnerabilities. This includes public web apps, exposed management interfaces, cloud storage end points, mail servers, and more.  
* Techniques: Several different techniques were utilized to formulate the findings for this report.   
  * OSINT reconnaissance was first implemented to help build a robust profile on Artemis, Inc. This includes technology Stack, personnel, email patterns, exposed services, subdomains, and public documents. External facing assets and open source information is disclosed. Below is what was utilized:  
    * Google Dorking: I used search engines like Google with targeted queries (ie, site:[artemis.com](http://artemis.com), filetype:pdf, inurl:login, “password” site:artemis.com) to discover indexed files, exposed portals, and sensitive content.  
    * Whois / Domain Tools: This is a service that displays domain registration and ownership information (registrant emails, name servers, creation/expiry dates)  
    * Nslookup / dig: Integrates the DNS in order to enumerate files and their extensions (ie. TXT, A, MX, SOA, NS)  
    * [crt.sh](http://crt.sh) (Certificate Transparency): Search certificate transparency logs to find subdomains and historical certs issued for artemis domains.  
    * Subdomain enumeration (Amass, subfinder): Automated discovery of subdomains by leveraging passive sources (OSINT feeds) and brute force when appropriate.  
    * Shodan: Search internet-connected devices and services (banners, ports, known CVEs).  
    * Censys: Alternative internet scan dataset to Shodan for discovering exposed hosts and certificates.  
    * The Harvester: Alternative internet scan dataset to Shodan for discovering exposed hosts and certificates.  
    * [Hunter.io](http://Hunter.io) / VoilaNorbert: Email pattern discovery and verification services to enumerate likely employee email addresses and formats.   
    * Maltego (Community/CE): Graph-based correlation tool to map relationships between domains, IPs, emails, and people.   
    * Recon-ng: A modular reconnaissance framework for automated OSINT collection and templated workflows.   
    * SpiderFoot: Automated OSINT reconnaissance platform that aggregates data from many public sources (DNS, SHODAN, WHOIS, social media).   
    * GitHub / GitLab code search: Search for leaked credentials, config files, or code referencing company domains or IPs.  
    * FOCA (Fingerprinting Organizations with collected Archives): Extract metadata from public documents (PDF, DOCX) to reveal usernames, network paths, or software versions.   
    * [Archive.org](http://Archive.org) (Wayback Machine): Review historical snapshots of public websites to find old endpoints, contact pages, or deprecated services.   
    * Linkedin / Twitter / Facebook: Gather employee names, roles, technology mentions, and potential resume information.  
    * ZoomEye / BinaryEdge: Additional internet search engines for exposed devices and services (like Shodan).  
    * Netcraft: Website and hosting technology profiling, including historical DNS and hosting changes.  
    * FOI / Government Filings / INPI / Patent Databases: Search patent submissions and public filings for PARS-related disclosures that may reveal technical details.   
    * Google Maps / Reverse WHOis / Passive DNS services: sources for obtaining physical addresses, related domains, and historical DNS activity.   
  * Identifying the targets and running scans was next in the procedure. Five tools or techniques were used for host discovery and network enumeration of Artemis’ external infrastructure. This is critical for establishing the attack surface, uncovering live hosts, open ports and associated services running on Artemis’ network. Listed below are the five tools and their uses:  
    * Nmap (Network Mapper): Nmap was used to scan Artemis’ known IP ranges to identify active hosts and their running services. It supports scripting (via NSE) for advanced enumeration (like banner grabbing or having an HTTP title)  
    * Fierce: Fierce was used to identify subdomains, internal ips, and possible misconfigured or shadow IT systems that are not centrally monitored. This supports host discovery beyond public facing IPs.  
    * OpenVAS: Once Nmap identified live hosts and open ports, OpenVAS was used to perform deeper service scans and fingerprint versions and vulnerabilities.  
    * Metasploit Framework: Metasploit was used after Nmap and OpenVAS to validate vulnerabilities and collect detailed information to produce a list of services installed and banner grabbing.  
    * Nikto: Was used to test Artemis' exposed web servers for outdated software, misconfigurations, default files, and insecure HTTP headers.  
  * Next, several tools were used to scan and detect vulnerabilities within Artemis, focusing on identifying vulnerabilities in the external infrastructure, web applications, and internal systems. These tools are:  
    * Tenable Nessus: Nessus was installed on Kali Linux. We will use the “Scan” feature for deep analysis. We will configure target IP ranges in the network, credentials, and plugins. Lastly, we will launch, scan and analyze results via the web UI. FIGURE 1  
<img width="988" height="481" alt="Screenshot 2026-04-01 153822" src="https://github.com/user-attachments/assets/9e941c31-a1ed-44ef-85d9-67c02f02e0d2" />
    * OpenVAS (Greenbone Vulnerability Management): We installed OpenVAS via Greenbone Vulnerability Management. After installing, we configured a scan task with the target IPs and scan configurations. We then reviewed reports generated by the scanner.
<img width="809" height="623" alt="Screenshot 2026-04-01 153933" src="https://github.com/user-attachments/assets/8fadb843-d943-402e-9613-d64794c90e9c" />
* Burp suite: We first set up Burp Proxy to intercept traffic  Next, crawled web applications and scanned for vulnerabilities such as XSS, SQLi, and CSRF. We will use the repeater to test specific payloads and responses.
<img width="927" height="486" alt="Screenshot 2026-04-01 154112" src="https://github.com/user-attachments/assets/1fafe99e-823b-4a97-8c38-7acb22568b92" />
    * Wapiti: Wapiti is a command-line vulnerability scanner for web applications. It was utilized in the following order:  
      * It can be installed via package manager.   
<img width="974" height="531" alt="Screenshot 2026-04-01 154223" src="https://github.com/user-attachments/assets/f873d72c-4dee-4969-9731-f881a97a5153" />
   * To run it, we used console from Linux and use the following command: wapiti http://targetsite.com \-u  
        * Targetsite is the generic name for whatever the target server host file will be  
      * We then analyzed the terminal output or report file.   <img width="922" height="561" alt="Screenshot 2026-04-01 154439" src="https://github.com/user-attachments/assets/fcc7d3e4-68c4-4e62-bc7b-7e14bed210f3" />

    * Nikto: Nikto is a classic open-source web server scanner that tests for bad files, outdated server software and other security issues.   
      * We ran the following command line in Console in Linux: nikto \-h [http://targetsite.com](http://targetsite.com)  
        * The target site is generic as an example of course.  
      * We then reviewed the output for alerts and risk indicators.
      * <img width="983" height="466" alt="Screenshot 2026-04-01 154623" src="https://github.com/user-attachments/assets/40f7a849-d3f6-41cb-82d1-41fedfd3cbb8" />

  * Within the scope of this testing report, several threats are found, assessed and modeled. Exploitation is discussed for its impact analysis and demonstration under Findings.   
  * Clean up of all work is also carried out at the end to prevent real hackers from using the same vulnerabilities.

2\.       **Project Objectives**  
This security assessment is carried out to gauge the security structure of Artemis’ internet facing hosts. We identify externally accessible vulnerabilities that could lead to data loss, service compromise, or lateral movement (cyber attacker moving “sideways” in compromised systems to delay detection  and to maintain access to systems) into sensitive systems, like APOLLO and PARS. Prioritized remediation and detection guidance is provided in technical detail. 

3	**Assumptions & Rules of Engagement**  
Artemis systems are hybrid, using AWS and multiple on-premise data centers. Findings are assessed in context of Artemis business assets, APOLLO and PARS. Findings are then evaluated with attack paths that a real malicious actor would take, combining available reconnaissance and likely privilege escalation chains. All activity and findings produced during the engagement were handled in accordance with confidentiality requirements from the contract.

**4	Timeline**

| Procedure in Penetration Test | Start Date | End Date |
| :---- | ----- | ----- |
| Pre-engagement phase | 09/09/2025 | 09/12/2025 |
| Reconnaissance phase | 9/13/2025 | 9/18/2025 |
| Mapping (to identify entry points) | 9/19/2025 | 9/21/2025 |
| Vulnerability scanning | 9/22/2025 | 9/25/2025 |
| Exploitation Phase | 9/27/2025 | 9/30/2025 |
| Clean Up | 10/01/2025 | 10/2/2025 |
| Reporting | 10/2/2025 | 10/4/2025 |

**5	Summary of Findings**

| Unpatched RDP Exposed to the Internet | 9.3 |
| :---- | ----: |
| Web Application Vulnerable to SQL Injection | 9.8 |
| Default Password on Cisco Admin Portal | 7.5 |
| Apache Web Server Vulnerable to CVE‑2019‑0211 | 7.8 |
| Web Server Exposing Sensitive Data | 6.5 |
| Web Application Has Broken Access Control | 8.6 |
| Oracle WebLogic Server Vulnerable to CVE‑2020‑14882 | 9.8 |
| Misconfigured Cloud Storage (AWS Security Group / S3 Misconfiguration) | 8.2 |
| Microsoft Exchange Server Vulnerable to CVE‑2021‑26855 (ProxyLogon) | 9.8 |

**Detailed Findings:**

* **Finding 1: Unpatched RDP Exposed to the internet**  
  * Remote Desktop is reachable from the internet on one or more hosts. TCP port 3389 is currently open and vulnerable on the Firewall. The majority of Windows versions accessed on Artemis’ network are older or are missing recent RDP/credential stuffing mitigations. These OS versions include: Windows 10 and 11; and Windows server 2008 R2, 2012, 2016, and 2019\.   
  * The suspected hosts were identified by utilizing Nmap to identify targets. Commands used from attack server:  
    * Nmap \- 3389 \-open \-sS \-Pn \-T4 \<target-range\>  
    * Nmap \-sV \-p 3389 –script=banner \<ip\>  
  * Impact: Successful compromise may allow credential harvesting, lateral movement, domain compromise and access to APOLLO and PARS.  
  * The Risk rating is critical, with a CVSS score ranging from **8.8 to 9.8**, depending on exploitability of each host and patch state.  
* **Finding 2: Web Applications Vulnerable to SQL Injection**  
  * Unsanitized input in RFQ or RFP web applications allow SQL query injection, revealing or manipulating backend data important to Artemis.  
  * Affected systems for this vulnerability would be the Linux Web Server.  
  * Evidence: Burp Suite intercepted traffic to identify injectable parameters. An example of the command ran is:  
    * sqlmap \-u "https://rfq.artemis.example/item?id=123" \--batch \--level=3 \--risk=2 \--dbs  
  * After exploitation, the following attack vectors can be carried out: Database exfiltration (contains Customer information, trade secrets and patent drafts), admin account creation, credentials harvested for lateral access to SAP/AD-integrated services. With SQL injections, WAF rules and DB activity logging can be utilized to block traffic for employees and clients. Threat actors can bypass access using blind SQL techniques and out-of-band exfiltration (DNS). A password cracking plan can easily be put in place for access, like SQLmap for extraction. If the password hashes are dumped, however, then Hashcat/John can be used with appropriate hash modes and targeted wordlists.  
  * The risk rating is critical, with a CVSS score of **9.8**.  
* **Finding 3: Default Password on Cisco Admin Portal**  
  * The default manufacturer credentials remain on the Cisco management interface. These were never changed upon installation. The password can be easily compromised. Software or sites like Hydra or Medusa can be used to view online login attempts with vendor default lists and targeted dictionaries  
  * Affected systems include Cisco Devices using iOS.  
  * Evidence:   
    * The following command can be ran to identify open ports: nmap \-p 22,23,80,443,8080 \--open \<example\_ip\_range\>  
    * The following can be ran to check for default credential checks only after permission is given: hydra \-L defaults.txt \-P smallpasslist.txt ssh://\<example\_ip\>  
  * Risks: Once a threat actor has this password, he/she has access to manipulate the network, including routing and ACLs. They can intercept packets, create VPN for persistent access, and manipulate logging on the cisco interface to hide activity.  
  * The risk rating is high, with a CVSS rating of **7.5**.  
* **Finding 4: Apache Web Server is Vulnerable to CVE-2019-0211**  
  * If on an older version of Apache for the web server, specifically older than 2.4.39, any local users on the domain or network have root access (as system admins essentially) via Privilege escalation.  
  * Risks:  
    * Any local user, existing or new (can be newly added if the server is already compromised), has access as a complete administrator to the server and possibly the domain.  
    * Install persistence can be implemented, which adds a dedicated storage space on the same USB drive to save data. This can allow an attacker to personalize the system, save files, and have desired settings ready each time the attacker boots from that USB drive.  
    * Configuration and credential files can be access and edited, and credentials can be cracked using the program /etc/shadow and John the Ripper and Hashcat Offline.

  The risk rating is high, with a CVSS rating of **7.8**.

* **Finding 5: The Web Server is Exposing Sensitive Data**  
  * There are publicly accessible directories and files exposing highly confidential information. This includes backup files, configuration files, environment notes, and much more. This affects Linux web hosts using Apache or Nginx.  
  * Risks:   
    * Discovery of API Keys  
    * Access to Database connection strings  
    * Access to admin credentials  
    * Access to Hosted Cloud access  
    * Direct Data Theft  
  * Evidence: These can be accessed simply by utilizing: Google dorking, direct URL checks, and certificate transparency to enumerate hosts.  
  * The Risk rating ranges from Medium to High, averaging a CVSS score of 6.5  
* **Finding 6: Web Application Has Broken Access Control**  
  * The Web server has insufficient authorization, allowing users to perform unauthorized actions or view restricted data. Systems affected are API’s and web pages owned and utilized by Artemis.  
  * Evidence: Manual parameter tampering in Burp, or in browser devtools in Chrome, were used to access other user resources.  
  * Risks:   
    * This allows unauthorized access to APOLLO/PARS data.  
    * Patent entries can be modified  
    * Admin features can be easily accessed  
    * Data can be tampered  
  * The risk rating is high, with a CVSS score of **8.6**.  
* **Finding 7: Oracle WebLogic Server Vulnerable to CVE‑2020‑14882**  
  * Unauthenticated RCE in WebLogic console leading to remote code execution on unpatched servers. The affected system is Oracle WebLogic.  
  * Evidence: Banner detection was used to see this update utilized.  
  * Risk: An authenticated, remote attacker can exploit this, via a specially crafted HTTP request, to execute arbitrary commands. This means the entire system can be compromised and attackers can implement lateral movement.  
  * Potential blocking mechanisms and bypass strategies: WAF, network segmentation, limiting console exposure. Bypass possible if the console is public and signatures are not tuned.  
  * Password cracking plan: Post‑exploit credential harvesting followed by offline cracking with Hashcat/John as needed.  
  * The Severity is high, with a CVSS rating of 9.8.  
* **Finding 8: Misconfigured Cloud Storage (AWS Security Group / S3 Misconfiguration)**  
  * Description of the vulnerability: Public S3 buckets or overly permissive security groups and IAM roles allowing data access or resource manipulation.  
  * Affected systems / OS / versions: AWS resources (S3, EC2 security groups, IAM roles).  
  * Risk and post-exploitation attack vectors: Exfiltrate proprietary artifacts, inject malicious content, use leaked keys to enumerate/modify cloud resources, pivot via exposed metadata/instance profiles.  
  * Potential blocking mechanisms and bypass strategies: GuardDuty, AWS Config, CloudTrail, IAM least privilege. Bypass involves abusing exposed keys or misconfigured roles on EC2 instances.  
  * Password cracking plan: If access keys obtained, test and enumerate; rotate and revoke exposed keys immediately.  
  * The Severity is high, with a CVSS rating of 8.2  
* **Finding 9: Microsoft Exchange Server Vulnerable to CVE-2021-26855 (ProxyLogon)**  
  * Description of the vulnerability: SSRF and RCE vulnerability in Exchange allowing unauthenticated access and code execution (ProxyLogon exploit chain).  
  * Affected systems / OS / versions: Microsoft Exchange Server 2013/2016/2019 on Windows Server hosts (unpatched).  
  * Risks of attempting to exploit: Exploitation can be disruptive to mail services and generate significant alerts.  
  * Risk and post-exploitation attack vectors: Mailbox compromise, credential harvesting, domain controller lateral moves, persistent webshells, mass exfiltration of emails and attachments.  
  * Potential blocking mechanisms and bypass strategies: EDR (Defender), network segmentation, emergency patching. Bypass is possible on unpatched, internet‑facing servers with public exploit code available.  
  * Password cracking plan: Use Mimikatz, CrackMapExec for harvesting; offline cracking with Hashcat for hashes if dumped.  
  * The Severity is high, with a CVSS rating of 9.8.

**6\.**	**Recommendations**

	Adopt an defense-in-depth approach where Artemis utilizes a variety of security tools and systems and processes. The following remediations are listed in the order of each finding listed in the previous section.

1. Unpatched RDP Exposed to the internet  
   1. Direct internet RDP should be disabled and VPN should be required for employees.  
   2. Enforce multi-factor authentication for users and a network access control list on the Server.  
   3. Apply Windows patches and keep them up-to-date  
   4. Require Network Level Authentication (NLA)  
   5. Apply monitoring, such as logging RDP connections, SIEM alerts on unusual geolocations, and new persistent endpoints that seem to continuously pop-up.  
2. Web Application Vulnerable to SQL Injection  
   1. Fix the code by using parameterized queries in SQL, or stored procedures. Validate and sanitize input server-side.  
   2. Harden Database accounts by applying least privilege access and read-only for the apps as often as possible.  
   3. Deploy and tune WAF (Web Application Firewall) with custom signatures and a positive security model.  
   4. Apply DAST (Dynamic Application Security Testing) and SAST (Static Application Security Testing). These find software vulnerabilities.  
3. Default Password on Cisco Admin Portal:  
   1. Change Default Credentials immediately on all systems and enforce strong password policies  
   2. Restrict management interfaces to internal management in a VLAN (Virtual Local Area Network)  
   3. Implement AAA framework and MFA (Mult-factor Authentication) for admin access.  
   4. Apply logging and configuration backups to detect unauthorized changes.  
4. Apache Web Server Vulnerable to CVE-2019-0211  
   1. Apply the latest Patch for Apache version immediately.  
   2. Make any web worker accounts as non-privileged accounts.  
   3. Apply SELinux or AppArmor that enforces mandatory access control policies.  
5. Web Server Exposing Sensitive Data  
   1. Remove Secrets from Webroot  
   2. Use secrets management, which is the process of securely storing, managing, and controlling access to sensitive information like passwords, API keys, and encryption keys  
   3. Rotate any encryption or API Keys  
   4. Audit any backups made  
6. Web Application having broken Access Control  
   1. Implement any strong server-side authorization checks  
   2. Enforce session management and access logging  
   3. Add automated access-control tests to Continuous Integration  
   4. Apply Role-Based Access Control (RBAC). This makes it easier to update a user’s access when their job function changes.  
7. Oracle WebLogic Server Vulnerable to CVE-2020-14882  
   1. Apply the latest patch for Oracle WebLogic server immediately  
   2. Restrict console to management or admin network  
   3. Implement WAF (Web Application Firewall) and network ACLs (essentially, rules for firewall)  
8. Misconfigured Cloud Storage (AWS Security Group)  
   1. Apply S3 Block Public Access, which helps prevent unintended public access to an entire AWS account or to individual S3 buckets  
   2. Audit IAM roles  
   3. Require MFA delete, which requires MFA to delete an object version for the Amazon S3 bucket.  
   4. Rotate any exposed keys detected  
   5. Enable logging (can use CloudTrail)  
9. Microsoft Exchange Server Vulnerable to CVE-2021-26855  
   1. Apply emergency Patching to CVE-2021-26855  
   2. Isolate Compromised server(s)  
   3. Threat hunt for webshells and suspicious mail rules.

**7\. 	References**

Cloudflare. (n.d.)*What are the security risks of RDP? | RDP Vulnerabilities.* Cloudflare. [https://www.cloudflare.com/learning/access-management/rdp-security-risks/](https://www.cloudflare.com/learning/access-management/rdp-security-risks/)  
Washington Information Technology.. (June 6, 2025). *Mitigating SQL Injection (SQLi) Vulnerabilities*. University of Washington. [https://it.uw.edu/guides/security-authentication/mitigating-sql-injection-sqli-vulnerabilities/](https://it.uw.edu/guides/security-authentication/mitigating-sql-injection-sqli-vulnerabilities/)

Cisco Advisories. (Feb, 19, 2020\) *Cisco Smart Software Manager On-Prem Static Default Credential Vulnerability*. Cisco. [https://sec.cloudapps.cisco.com/security/center/content/CiscoSecurityAdvisory/cisco-sa-on-prem-static-cred-sL8rDs8](https://sec.cloudapps.cisco.com/security/center/content/CiscoSecurityAdvisory/cisco-sa-on-prem-static-cred-sL8rDs8)

Tenable. (April 8, 2019\) *CVE-2019-0211: Proof of Concept for Apache Root Privilege Escalation Vulnerability Published*. Tenable. [https://www.tenable.com/blog/cve-2019-0211-proof-of-concept-for-apache-root-privilege-escalation-vulnerability-published](https://www.tenable.com/blog/cve-2019-0211-proof-of-concept-for-apache-root-privilege-escalation-vulnerability-published)

OWASP. (n.d.) *A01:2021 \- Broken Access Control*. OWASP. [https://owasp.org/Top10/A01\_2021-Broken\_Access\_Control/](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)

Tenable. (Nov 6, 2020\) *Oracle WebLogic Server RCE (CVE-2020-14882)*. Tenable. [https://www.tenable.com/plugins/nessus/142594](https://www.tenable.com/plugins/nessus/142594)

Cycode. (March 1, 2022\) *How to Prevent AWS S3 Bucket Misconfigurations.* Cycode. [https://cycode.com/blog/how-to-prevent-aws-s3-bucket-misconfigurations/](https://cycode.com/blog/how-to-prevent-aws-s3-bucket-misconfigurations/)

Vipre Security Group. (n.d.) *What is the ProxyLogon Vulnerability?* Vipre. [https://vipre.com/glossary-terms/what-is-proxylogon-vulnerability/?srsltid=AfmBOoogG3m2UF828Pjmi-ZIi1q2Q77CAZ9ccAsGLYkt1htsVrip2yKH](https://vipre.com/glossary-terms/what-is-proxylogon-vulnerability/?srsltid=AfmBOoogG3m2UF828Pjmi-ZIi1q2Q77CAZ9ccAsGLYkt1htsVrip2yKH)
