# 🛡️ Home SOC Lab — Live Threat Detection with Telegram Alerts & Web Dashboard

A hands-on Security Operations Center (SOC) home lab built on virtual machines that detects network attacks in real time, sends instant alerts via a Telegram bot, and visualizes live events on a web dashboard.

---

## 🎯 Objective

The goal of this project is to simulate a real-world SOC environment using virtual machines. The lab monitors network traffic between an attacker and a victim machine, detects malicious activity using an IDS (Intrusion Detection System), and delivers **live alerts** through two channels:

- 📲 **Telegram Bot** — Instant push notifications when an attack is detected
- 🌐 **EveBox Web Dashboard** — Live visual feed of all triggered alerts

This project demonstrates how a SOC analyst can set up threat detection infrastructure from scratch and respond to attacks in real time.

---

## 🧠 Skills Learned

- Setting up and managing a multi-VM lab environment (attacker, victim, SOC analyst)
- Configuring and running **Suricata** as a Network Intrusion Detection System (NIDS)
- Analysing and interpreting **eve.json** log output from Suricata
- Setting up a vulnerable target machine (DVWA + MariaDB)
- Building a **Python script** to parse live logs and push alerts to Telegram
- Using **EveBox** for real-time alert dashboarding
- Understanding attack patterns such as ping sweeps, SQL injection, brute force, etc.
- Network communication between VMs using host-only/bridged adapters

---

## 🛠️ Tools Used

| Tool | Description |
|------|-------------|
| **Kali Linux (Attacker)** | The offensive machine used to simulate attacks against the victim |
| **Kali Linux (Victim)** | The target machine made intentionally vulnerable for testing |
| **Parrot OS (SOC)** | The analyst/monitoring machine running all detection and alerting tools |
| **Suricata** | Open-source IDS/IPS that monitors network traffic and generates alerts based on rule signatures |
| **EveBox** | Web-based dashboard that reads Suricata's `eve.json` and displays alerts in a clean UI |
| **DVWA** | Damn Vulnerable Web Application — a deliberately insecure PHP/MySQL web app used as the attack target |
| **MariaDB** | Database server installed on the victim machine to support the DVWA application |
| **Python** | Used to write a script that tails the `eve.json` file and forwards new alerts to Telegram |
| **Telegram Bot API** | Receives parsed alert data from the Python script and delivers live push notifications |

---

## 🔬 Steps

### Step 1 — Network Verification: Ping All Three VMs

Verified that all three virtual machines (Kali Attacker, Kali Victim, Parrot OS SOC) can communicate with each other over the network before proceeding.

- ping from victim to SOC
 <img width="1920" height="1080" alt="pinging from victim to parrot" src="https://github.com/user-attachments/assets/9a7fbf71-cda6-4d83-81df-5482fee7a158" />


- ping from victim to attacker
 <img width="1920" height="999" alt="pingning from victim to the attacker" src="https://github.com/user-attachments/assets/b5b4272b-74dc-4b36-ad84-8e6483d47f55" />


- ping from attacker to SOC
<img width="1920" height="1080" alt="pinging from attacker to parrot os" src="https://github.com/user-attachments/assets/11114622-7629-4ac6-a2d2-0c095790eb02" />


- ping from attacker to victim
<img width="1920" height="993" alt="pinging from attacker to the victim" src="https://github.com/user-attachments/assets/6bbce987-7f2b-4ead-8e45-3bf6a2e2ca4b" />


- ping from SOC to attacker
 <img width="1920" height="1080" alt="pinging from parrot to attacker 1" src="https://github.com/user-attachments/assets/31f0639d-80c7-43f8-b671-273abcb44238" />


- ping from SOC to victim
 <img width="1920" height="1080" alt="pinging from parrot to victim k2" src="https://github.com/user-attachments/assets/539cfa42-f929-4515-8544-e059de71b730" />


  

---

### Step 2 — Making the Victim Machine Vulnerable

Configured the Kali Linux victim machine to be intentionally vulnerable by adding new user accounts and weakening its security posture to allow attack simulations.



### Step 3 — Installing MariaDB Server & DVWA

Installed **MariaDB** as the database backend and set up **DVWA (Damn Vulnerable Web Application)** on the victim machine to serve as a live target for web-based attacks.

> 📸 *Screenshot — DVWA running in browser on victim machine*

---

### Step 4 — Installing Suricata on Parrot OS (SOC Machine)

Installed and configured **Suricata** on the Parrot OS machine to monitor network traffic flowing between the attacker and victim. Suricata logs all detected threats to the `eve.json` file.

> 📸 *Screenshot — Suricata running and monitoring traffic*

---

### Step 5 — Setting Up EveBox Web Dashboard

Installed and launched **EveBox**, which reads Suricata's `eve.json` in real time and presents all alerts on a live, searchable web dashboard.

> 📸 *Screenshot — EveBox dashboard showing live alerts*

---

### Step 6 — Python Script: Live Telegram Alert Notifications

Wrote a **Python script** that continuously monitors the `eve.json` file for new Suricata events. When an attack is detected, the script parses the alert details and sends an instant notification to a configured **Telegram bot**.

> 📸 *Screenshot — Python script running + Telegram alert received*

---

## 📁 Project Structure

```
soc-home-lab/
├── alert_bot.py          # Python script for Telegram alerts
├── screenshots/          # Step-by-step lab screenshots
└── README.md
```

---

## ⚠️ Disclaimer

This lab is built for **educational and ethical purposes only**. All attacks are performed in an isolated virtual environment with no connection to real-world systems or networks.
