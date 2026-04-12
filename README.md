# 🔧 it-support-playbooks

> A structured IT troubleshooting knowledge base built from real-world support scenarios — documenting diagnostic processes, tools used, and verified resolutions.

All playbooks are based on live production environments. Screenshots included where available.

---

## 📂 Playbook index

### 🖥️ Remote support

| Playbook | Tools | Description |
|---|---|---|
| [Remote Linux update via TeamViewer](remote-support/remote-linux-update-via-teamviewer.md) | TeamViewer, Ubuntu terminal | Establishing a remote session, verifying network interfaces, running apt updates safely on a remote machine |

---

### 🌐 Networking

| Playbook | Tools | Description |
|---|---|---|
| [Network monitoring & alarm response](networking/network-monitoring-alarm-response-ruijie-reyee.md) | Ruijie Reyee, Neteak NMS | Responding to incoherent power alarms, analysing hotel network topology, Wi-Fi experience diagnostics |

---

### 📞 VoIP / PBX

| Playbook | Tools | Description |
|---|---|---|
| [UCM6202 PBX administration](voip-pbx/ucm6202-pbx-extension-and-outbound-routing.md) | Grandstream UCM6202, Telone SIP trunk | System health checks, SIP extension provisioning, outbound dial pattern configuration for Zimbabwean mobile and international calls |

---

### 🪟 Windows troubleshooting

| Playbook | Tools | Description |
|---|---|---|
| [Event Viewer — Event 1014 & 10016](windows-troubleshooting/windows-event-viewer-1014-10016-diagnostics.md) | Windows Event Viewer, PowerShell | Diagnosing DNS Client timeout and DCOM permission warnings on Windows 11, distinguishing startup noise from real issues |

---

## 🗂️ Repository structure

```
it-support-playbooks/
│
├── remote-support/
│   ├── assets/                  # Screenshots from live sessions
│   └── remote-linux-update-via-teamviewer.md
│
├── networking/
│   ├── assets/
│   └── network-monitoring-alarm-response-ruijie-reyee.md
│
├── voip-pbx/
│   ├── assets/
│   └── ucm6202-pbx-extension-and-outbound-routing.md
│
└── windows-troubleshooting/
    ├── assets/
    └── windows-event-viewer-1014-10016-diagnostics.md
```

---

## 🛠️ Tools & platforms documented

![TeamViewer](https://img.shields.io/badge/TeamViewer-0E8EE9?style=flat-square&logo=teamviewer&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=flat-square&logo=linux&logoColor=black)
![Windows](https://img.shields.io/badge/Windows-0078D6?style=flat-square&logo=windows&logoColor=white)
![PowerShell](https://img.shields.io/badge/PowerShell-5391FE?style=flat-square&logo=powershell&logoColor=white)
![Wireshark](https://img.shields.io/badge/Wireshark-1679A7?style=flat-square&logo=wireshark&logoColor=white)
![Ubiquiti](https://img.shields.io/badge/Ubiquiti%20UniFi-0559C9?style=flat-square&logo=ubiquiti&logoColor=white)

---

## 📋 Playbook template

Every playbook in this repo follows a consistent structure:

```
## Overview         — environment, tools, skill level
## Scenario         — what the situation was
## Screenshots      — real captures from live sessions
## Diagnostic steps — step-by-step investigation
## Resolution       — what fixed it and why
## Common issues    — related problems and quick fixes
## Best practices   — how to prevent recurrence
```

---

## 👩‍💻 About

Built by **Tanyaradzwa Jimu** — IT Support Specialist based in Harare, Zimbabwe.  
Documenting real-world IT support work across networking, remote support, VoIP, and Windows administration.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/tanyajimu)
[![Upwork](https://img.shields.io/badge/Upwork-6FDA44?style=flat-square&logo=upwork&logoColor=white)](https://www.upwork.com/freelancers/~01395571bc0dc02786)

> *More playbooks added regularly as new scenarios are encountered in production.*
