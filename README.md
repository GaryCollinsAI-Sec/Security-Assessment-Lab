# Controlled Pentesting Lab: Exploiting & Hardening Legacy Systems

## Objective


To design and deploy a segmented network environment for the safe execution, analysis, and remediation of legacy exploits. Using pfSense as a firewall/router, this lab simulates a real-world enterprise DMZ where a Kali Linux attacker must bypass network controls to exploit known vulnerabilities in a Metasploitable 2 target. Beyond exploitation, the project focuses on defense-in-depth, documenting the process of hardening legacy systems and implementing pfSense-level mitigations to neutralize threats without breaking business functionality.

### Skills Learned


- Network Segmentation: Designing and implementing multi-subnet architectures using VLANs in pfSense to isolate high-risk legacy assets.
- Firewall Administration: Configuring Stateful Packet Inspection (SPI), firewall rules (Allow/Deny), and NAT translation to enforce the principle of least privilege.
- DMZ Architecture: Simulating real-world enterprise environments by placing vulnerable targets behind a security gateway.
- Penetration Testing Methodology: Executing a structured attack lifecycle (Reconnaissance → Scanning → Exploitation → Post-Exploitation) using Kali Linux and Metasploit.
- Legacy System Exploitation: Understanding the mechanics of "classic" vulnerabilities ( SMB exploits, outdated CGI scripts) and why they remain threats in modern networks.
- Attack Surface Reduction: Disabling non-essential services and protocols (FTP, Telnet, Rsh) to harden a "noisy" target.
- Compensating Controls: Implementing "Virtual Patching" via firewall rules when direct software patching is not possible for legacy code.
- System Hardening: Applying host-level security configurations, including credential strengthening and disabling root-level access.
- Virtual Lab Management: Deploying and interconnecting diverse Operating Systems (Ubuntu, Kali, BSD-based pfSense) within a virtualized hypervisor.
- Troubleshooting: Diagnosing routing issues and connectivity problems across a segmented virtual network.

### Tools Used




## Steps
drag & drop screenshots here or use imgur and reference them using imgsrc


