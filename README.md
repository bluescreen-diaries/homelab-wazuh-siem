# homelab-wazuh-siem

A hands-on SIEM homelab using Wazuh, Docker, and Proxmox.

This project is part of my ongoing homelab journey while transitioning into IT and cybersecurity. I built it to get more practical experience with log analysis, alerting, and threat detection in a real environment instead of only learning from coursework or certification material.

---

## why Wazuh

I wanted something I could realistically run on my own hardware without paying for expensive licensing.

Wazuh checked a lot of boxes for me:

* open source
* widely used for SIEM/SOC learning
* solid documentation
* Docker-friendly
* works well in smaller homelab environments

It also gave me a chance to work more with Linux, networking, agents, and troubleshooting across multiple systems.

---

## hardware

| Component | Spec                                                |
| --------- | --------------------------------------------------- |
| Machine   | Lenovo ThinkCentre M720s                            |
| CPU       | Intel Core i7-8700 @ 3.20GHz (6 cores / 12 threads) |
| RAM       | 16GB DDR4                                           |
| Storage   | 256GB SSD                                           |

Running Proxmox VE as the hypervisor with multiple VMs on top.

---

## stack

* Wazuh (manager + dashboard) deployed with Docker Compose
* Proxmox VE as the hypervisor
* Ubuntu VM hosting the Docker environment
* Windows Server VM acting as an Active Directory/log source
* Linux VM as a secondary log source
* Kali Linux for generating test activity

---

## architecture

```text
Proxmox Host
├── Ubuntu VM (Docker)
│   ├── Wazuh Manager
│   └── Wazuh Dashboard
├── Windows Server VM (AD)
│   └── Wazuh Agent
├── Linux VM
│   └── Wazuh Agent
└── Kali Linux VM (testing)
```

Wazuh agents installed on the Windows and Linux VMs forward logs back to the manager. The dashboard runs on the Ubuntu VM and is accessible across the local network.

---

## setup

### 1. Provision the Ubuntu VM on Proxmox

* 6GB RAM
* 50GB disk
* Install Docker + Docker Compose

### 2. Deploy Wazuh

```bash
git clone https://github.com/wazuh/wazuh-docker.git
cd wazuh-docker/single-node
docker compose up -d
```

After startup, the dashboard becomes available at:

```text
https://<vm-ip>
```

### 3. Install agents

Install the Wazuh agent on:

* Windows Server VM
* Linux VM

Once connected, both systems should appear in the dashboard under Agents.

### 4. Generate test events

Some testing I used:

* failed SSH login attempts
* port scans from Kali
* Windows authentication events
* service start/stop events

Watching alerts populate in real time was honestly one of the coolest parts of the setup.

---

## what I’m learning

* Reading and correlating logs across Windows and Linux systems
* Understanding how SIEM alerting actually works in practice
* Troubleshooting agents, networking, and Linux services
* Mapping alerts to MITRE ATT&CK techniques
* Getting more comfortable documenting technical projects

---

## notes

This is a personal learning project built as part of my homelab and IT transition journey.

I used AI tools like Claude for setup guidance, troubleshooting help, and documentation support throughout the process. I prefer being transparent about that because it reflects how I actually learn and work. I am an AI supporter as long as all the work done is ethical and transperent.
