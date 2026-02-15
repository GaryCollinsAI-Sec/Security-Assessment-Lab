<h1>Security Assessment: Legacy System Exploitation & Remediation</h1>

<h2>Objective</h2>
<p>
Developed a secure, sandboxed environment to simulate the full attack lifecycle against legacy systems. To ensure safe testing, I deployed a pfSense virtual machine to isolate the Metasploitable 2 instance from the host network, creating a controlled DMZ for the assessment. This project demonstrates proficiency in reconnaissance, vulnerability validation, and risk mitigation. By targeting Metasploitable 2 from a Kali Linux node, I analyzed service-level vulnerabilities and engineered a Remediation Roadmap involving host-level hardening and network-level compensating controls via the pfSense firewall.
</p>

<hr>

<h2>Skills Learned</h2>
<ul>
  <li>Network Security: VLAN segmentation, pfSense SPI firewall administration, DMZ architecture design</li>
  <li>Vulnerability Management: Identify classic legacy vulnerabilities and implement compensating controls (virtual patching)</li>
  <li>Ethical Hacking Lifecycle: Conduct methodical 5-phase assessment using Kali Linux and Metasploitable 2</li>
  <li>Defensive Hardening: Reduce attack surface via service minimization and Principle of Least Privilege enforcement</li>
  <li>Security Documentation: Translate exploit validation into technical reports and executive summaries</li>
  <li>Systems Engineering: Hypervisor management and cross-platform troubleshooting (Linux and BSD)</li>
</ul>

<hr>

<h2>Tools Used</h2>
<ul>
  <li>Kali Linux (attacker node)</li>
  <li>Metasploitable 2 (vulnerable target)</li>
  <li>pfSense</li>
  <li>Oracle VirtualBox</li>
  <li>Netcat(nc)</li>
  <li>Netdiscover</li>
</ul>

<hr>

<h2>Implementation Steps</h2>


<h3>Phase 1: Reconnaissance & Host Discovery</h3>


<p>
This image displays the results of an active ARP (Address Resolution Protocol) scan using Netdiscover on the Kali Linux attacker node.

Host Discovery: The tool captured ARP Request/Reply packets to identify active hosts within the local subnet without relying on ICMP (ping), which is often blocked by firewalls.

Target Identification: By analyzing the MAC Vendor (PCS Systemtechnik GmbH), I was able to identify the virtualized interfaces belonging to the Metasploitable 2 and pfSense instances within the VirtualBox environment.

Outcome: This step provided the target IP addresses necessary to begin the scanning and enumeration phase with Nmap.</p>





<img width="629" height="427" alt="Edit netdiscover" src="https://github.com/user-attachments/assets/dfeaf01a-7515-4fa0-800f-69695f87796e" />



<h3>Phase 2: Vulnerability Validation (The vsFTPd Backdoor)</h3>



Attack Vector: Exploited a known backdoor in the vsFTPd v2.3.4 service that triggers a listener on port 6200 when a specific string (:)) is sent in the username field.

Technical Execution: Used Netcat to manually interact with the service on port 21. By delivering the "smiley face" payload, I successfully forced the application to execute a hidden shell listener.

Impact: Validated unauthenticated Root Access, allowing for full system compromise.



<img width="283" height="113" alt="netcat edit" src="https://github.com/user-attachments/assets/fe3b8b6e-806f-4d6a-bf49-21e0d6e51e45" />




<h3> Phase 3: Exploitation & Post-Exploitation </h3>

After triggering the backdoor, I established a connection to the shell listener on port 6200. To demonstrate full system command, I performed the following:

Identity Verification: Executed whoami and id to confirm the shell was running with Root (UID 0) privileges.

System Enumeration: Gathered hostname and network configuration data to map the internal environment.

Full System Control (The "Kill Chain" Proof): As the final validation of unauthenticated root access, I issued the reboot command.

Impact: Successfully forcing a system-wide reboot from a remote shell proves that an attacker has the power to cause a Permanent Denial of Service (PDoS) or clear volatile system logs (/var/log) to erase their digital footprint before the system comes back online.


<img width="561" height="128" alt="netcat2 edit" src="https://github.com/user-attachments/assets/e81c3784-a438-45db-8a25-a00b02a15204" />



<h3> Phase 4:  Maintaining Access (Persistence) </h3>


After validating root access via a system reboot in Phase 3, I analyzed how an adversary would typically maintain a foothold. While not executed in this specific lab to maintain environment integrity, the following persistence mechanisms were identified as high-risk:

Credential Harvesting: Extracting the /etc/shadow file to perform offline brute-force attacks on user passwords.

Authorized Keys Injection: Appending an attacker-controlled RSA public key to /root/.ssh/authorized_keys for persistent, encrypted remote access.

Scheduled Tasks: Creating a crontab entry to trigger a reverse shell back to the Kali node every 60 minutes.




<h3> Phase 5: Remediation & Hardening (The "Blue Team" Response) </h3>

<img width="693" height="319" alt="Screenshot 2026-02-15 123646" src="https://github.com/user-attachments/assets/cd9e0573-eade-454a-a1b2-20adc8f2ecc2" />


- Once the system was back online, I transitioned to a defensive posture to ensure the backdoor could no longer be triggered:

- Network-Level: Configured pfSense Firewall rules to block traffic on Port 21 (FTP) and Port 6200 (Backdoor).

- Host-Level: Disabled the vsftpd service entirely to reduce the attack surface.

- Verification: Attempted the Netcat exploit again; the connection was Refused, confirming the fix was successful.

## In Summary:

To transition from an offensive to a defensive posture, I developed a Remediation Roadmap based on a Defense-in-Depth strategy. While the lab focused on exploit validation, a professional deployment would involve implementing compensating controls. Such as a pfSense "Virtual Patch" to block Ports 21 and 6200 allowing a host-level service minimization. This approach ensures that even if a legacy service cannot be immediately patched, the attack surface is neutralized at the network perimeter.
