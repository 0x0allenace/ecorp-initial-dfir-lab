# 🛡️ ECorp DFIR Lab Simulation

## Overview
The **ECorp DFIR Lab Simulation** is a hands-on cybersecurity lab designed to replicate a small enterprise network for **Digital Forensics and Incident Response (DFIR)**.

This project simulates a corporate LAN environment using:
- Active Directory infrastructure
- Network segmentation with pfSense
- Endpoint visibility with Velociraptor
- Adversary emulation using Kali Linux

The lab provides a controlled environment for practicing **incident detection, investigation, and response workflows**.

## Objectives

- Build a realistic enterprise DFIR lab
- Simulate attacker activity within a corporate network
- Practice log collection, analysis, and forensic investigation
- Develop structured incident response workflows
- Understand endpoint and network-level visibility

## Lab Architecture

The simulated environment includes:

- **pfSense Firewall/Router** → Network segmentation and traffic control  
- **Windows Active Directory** → Domain controller and user management  
- **Velociraptor Server/Agents** → Endpoint visibility and forensic collection  
- **Kali Linux Attacker Machine** → Adversary simulation  

## Use Cases

- Incident response simulation  
- Threat hunting practice  
- DFIR workflow development  
- Malware investigation (safe environment)  
- Log analysis and correlation  

## 📂 Repository Structure

```text
ECorp-Initial-DFIR-Lab-Simulation/
│
├── docs/        # Lab setup guides and topology documentation
├── configs/     # Velociraptor and pfSense configurations
├── templates/   # Incident response checklists and templates
└── README.md
```

## Setup Guide

Detailed setup instructions are available in the `/docs` directory.

### Basic Steps:
1. Set up pfSense for network routing and segmentation  
2. Deploy Windows Server and configure Active Directory  
3. Install Velociraptor server and agents  
4. Configure endpoints and logging  
5. Launch Kali Linux for attack simulation  

## DFIR Workflow

This lab enables:

- Detection of suspicious activity  
- Endpoint data collection via Velociraptor  
- Log analysis and timeline reconstruction  
- Investigation of attacker behavior  
- Incident documentation and reporting  

## Included Resources

- Lab topology and setup documentation (`/docs`)  
- Configuration files (`/configs`)  
- Incident response templates (`/templates`)  


## Future Improvements

- [ ] Add SIEM integration (Splunk / ELK)  
- [ ] Simulate real-world attack scenarios (lateral movement, persistence)  
- [ ] Add automated detection rules  
- [ ] Expand network size and segmentation  
- [ ] Include sample forensic datasets  

## 👨🏾‍💻 Author

**Allen Ace**  
Cybersecurity | DFIR | Threat Analysis | Machine Learning 

- GitHub: https://github.com/0x0allenace  
- LinkedIn: https://linkedin.com/in/allen-ace-soc-analyst  
- X: https://x.com/allen_acee  

## License
MIT License
