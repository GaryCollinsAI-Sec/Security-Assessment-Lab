<h1>Security Assessment Lab</h1>

<h2>Objective</h2>
<p>
    Developed a sandboxed environment to simulate the full attack lifecycle against legacy systems. 
    This project demonstrates proficiency in <b>reconnaissance, vulnerability validation, and risk mitigation</b>. 
    By targeting Metasploitable 2 from a Kali Linux node, I analyzed service-level vulnerabilities 
    and engineered a <b>Remediation Roadmap</b> involving host-level hardening and network-level 
    compensating controls via a pfSense firewall.
</p>

<hr>

<h2>Skills Learned</h2>
<ul>
  <li>Network Security: VLAN segmentation, pfSense SPI firewall administration, DMZ architecture design</li>
  <li>Vulnerability Management: Identify classic legacy vulnerabilities and implement compensating controls (virtual patching)</li>
  <li>Ethical Hacking Lifecycle: Conduct methodical 5-phase assessment using Kali Linux and Metasploit</li>
  <li>Defensive Hardening: Reduce attack surface via service minimization and Principle of Least Privilege enforcement</li>
  <li>Security Documentation: Translate exploit validation into technical reports and executive summaries</li>
  <li>Systems Engineering: Hypervisor management and cross-platform troubleshooting (Linux and BSD)</li>
</ul>

<hr>

<h2>Tools Used</h2>
<ul>
  <li>Kali Linux (attacker node)</li>
  <li>Metasploitable 2 (vulnerable target)</li>
  <li>pfSense (firewall and network segmentation)</li>
  <li>VirtualBox / VMware (hypervisor environment)</li>
  <li>Metasploit Framework (exploitation and post-exploitation)</li>
  <li>Various Linux/BSD/Windows tools for host-level hardening and testing</li>
  <li>Netcat(nc): The "Swiss Army Knife" of TCP/IP, used for raw manual connections and shell listeners.</li>
  <li>Netdiscover: Active/passive Address Resolution Protocol (ARP) reconnaissance for host discovery.</li>
</ul>

<hr>

<h2>Implementation Steps</h2>


<h3>Phase 1: Reconnaissance</h3>
<img width="629" height="427" alt="Edit netdiscover" src="https://github.com/user-attachments/assets/dfeaf01a-7515-4fa0-800f-69695f87796e" />

<p>
This image displays the results of an active ARP (Address Resolution Protocol) scan using Netdiscover on the Kali Linux attacker node.

Host Discovery: The tool captured ARP Request/Reply packets to identify active hosts within the local subnet without relying on ICMP (ping), which is often blocked by firewalls.

Target Identification: By analyzing the MAC Vendor (PCS Systemtechnik GmbH), I was able to identify the virtualized interfaces belonging to the Metasploitable 2 and pfSense instances within the VirtualBox environment.

Outcome: This step provided the target IP addresses necessary to begin the scanning and enumeration phase with Nmap.</p>

<h3>Phase 2: Vulnerability Validation</h3>



Attack Vector: Exploited a known backdoor in the vsFTPd v2.3.4 service that triggers a listener on port 6200 when a specific string (:)) is sent in the username field.

Technical Execution: Used Netcat to manually interact with the service on port 21. By delivering the "smiley face" payload, I successfully forced the application to execute a hidden shell listener.

Impact: Validated unauthenticated Root Access, allowing for full system compromise.


<h3> Phase 3: Post-Exploitation (Situational Awareness)</h3>

Action: Once "root" is gained, what does the attacker do? This involves searching for sensitive files (/etc/shadow), checking network configurations, or looking for database credentials.

Screenshot: Terminal showing whoami (root) and perhaps a cat /etc/passwd to show system users.

Key Tool: Basic Linux commands (id, ls -la, ifconfig).


<h3> Phase 4: Maintaining Access (Persistence) </h3>


Action: Ensuring you can get back into the system without re-exploiting the backdoor.

Screenshot: Adding a new administrative user or an SSH key to the target.

Key Tool: User management commands (useradd) or Netcat listeners.


<h3>Phase 5: Covering Tracks (Antiforensics)</h3>

Action: Deleting command history and system logs to avoid detection by an administrator or an Incident Response team.

Screenshot: Deleting .bash_history or clearing /var/log/ entries.

Key Tool: history -c, shred.


<h3>Phase 6: Remediation & Hardening (The "Blue Team" Phase)</h3>


Action: Implementing the "Remediation Roadmap."

Screenshot: pfSense firewall rules blocking Port 21 or Port 6200, or a screenshot of the service being disabled.

Key Tool: pfSense WebGUI, Linux service management (systemctl stop).




