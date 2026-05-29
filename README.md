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

- pinging from victim to SOC
 <img width="1920" height="1080" alt="pinging from victim to parrot" src="https://github.com/user-attachments/assets/9a7fbf71-cda6-4d83-81df-5482fee7a158" />


- pinging from victim to attacker
 <img width="1920" height="999" alt="pingning from victim to the attacker" src="https://github.com/user-attachments/assets/b5b4272b-74dc-4b36-ad84-8e6483d47f55" />


- pinging from attacker to SOC
<img width="1920" height="1080" alt="pinging from attacker to parrot os" src="https://github.com/user-attachments/assets/11114622-7629-4ac6-a2d2-0c095790eb02" />


- pinging from attacker to victim
<img width="1920" height="993" alt="pinging from attacker to the victim" src="https://github.com/user-attachments/assets/6bbce987-7f2b-4ead-8e45-3bf6a2e2ca4b" />


- pinging from SOC to attacker
 <img width="1920" height="1080" alt="pinging from parrot to attacker 1" src="https://github.com/user-attachments/assets/31f0639d-80c7-43f8-b671-273abcb44238" />


- pinging from SOC to victim
 <img width="1920" height="1080" alt="pinging from parrot to victim k2" src="https://github.com/user-attachments/assets/539cfa42-f929-4515-8544-e059de71b730" />


  

---

### Step 2 — Making the Victim Machine Vulnerable

Configured the Kali Linux victim machine to be intentionally vulnerable by adding new user accounts and weakening its security posture to allow attack simulations.

- adding users with weak passwords
<img width="1920" height="1080" alt="adding users with weak passwords" src="https://github.com/user-attachments/assets/f00dffeb-e40f-484d-aa5e-b555b10c6360" />

- adding users with weak passwords
<img width="1920" height="1080" alt="adding another user" src="https://github.com/user-attachments/assets/1a6d025c-0d80-435a-b783-97eebe333441" />



### Step 3 — Installing MariaDB Server & DVWA

Installed **MariaDB** as the database backend and set up **DVWA (Damn Vulnerable Web Application)** on the victim machine to serve as a live target for web-based attacks.

- installing dependencies (git, php, php-mysqli)
  <img width="1920" height="1080" alt="installing dependories github" src="https://github.com/user-attachments/assets/40ce325b-d22e-4760-bdff-669ad4e8cb2d" />

- Cloning DVWA git
  <img width="1920" height="1080" alt="cloning github" src="https://github.com/user-attachments/assets/5d649849-6a29-4c1d-850e-39ba77bfedec" />

- enabling apache 2 server and mysql server
  <img width="1920" height="1080" alt="enabling apache 2 github" src="https://github.com/user-attachments/assets/7c5824ae-df44-47a2-babb-7148f98bdd08" />

  <img width="1920" height="1080" alt="enabling mysql githuib" src="https://github.com/user-attachments/assets/b7452531-7786-498f-affd-9f79569a11ed" />

- configuring DVWA config
<img width="1920" height="1080" alt="before configuring the php github" src="https://github.com/user-attachments/assets/68c56ad1-b1d8-4862-99a2-4b7e6de88cd5" />

- configuring mariadb by adding a user that can access thorugh local host
<img width="1920" height="1080" alt="configuring mariadb by adding a user that can accessibale thorugh local host" src="https://github.com/user-attachments/assets/b5964bb1-cf80-4247-a1eb-ed49e79c7392" />

- DVWA login page and web page
<img width="1920" height="1080" alt="DVWA login page" src="https://github.com/user-attachments/assets/8211538d-0806-45bf-86d5-d9a759c46414" />

<img width="1920" height="1080" alt="DVWA page" src="https://github.com/user-attachments/assets/36b9a6a2-f69a-4278-9f92-cfc9bd3fbfd2" />

### Step 4 — Installing Suricata on Parrot OS (SOC Machine)

Installed and configured **Suricata** on the Parrot OS machine to monitor network traffic flowing between the attacker and victim. Suricata logs all detected threats to the `eve.json` file.

- installing suricata on parrot OS
<img width="1920" height="1080" alt="installing suricata" src="https://github.com/user-attachments/assets/018c64bb-7f2f-47fd-950f-f66e8d1c6859" />

- configuring suricata by adding the network interface (Network of the SOC lab) and local rules
<img width="1920" height="1080" alt="configuring suricata" src="https://github.com/user-attachments/assets/ef95013e-65d7-4783-ac0d-928dc6f943f6" />

- updating the configuration of the suricata.yaml ( local rules and network)
<img width="1920" height="1080" alt="updating suricat rules" src="https://github.com/user-attachments/assets/ee6ff190-9948-4a15-8ce7-bb1b717435ee" />

- testing suricata fast log ( which is only suitable for normal monitoring) by doing a nmap scan from attacker vm to victim vm
<img width="1920" height="1080" alt="nmap scan from ali 1 to kali 2 suricata" src="https://github.com/user-attachments/assets/a57f08f4-9b8a-412b-b6ac-561620e3d521" />

<img width="1920" height="1080" alt="suricata detecting nmap scans from kali1 to kali 2" src="https://github.com/user-attachments/assets/e7dfd5b9-1acf-4441-b474-6b2ab48bae70" />

### Step 5 — Setting Up EveBox Web Dashboard

Installed and launched **EveBox**, which reads Suricata's `eve.json` in real time and presents all alerts on a live, searchable web dashboard.

- installing evebox web dashboard
<img width="1920" height="1080" alt="new installing evebox" src="https://github.com/user-attachments/assets/2f0b91fe-5b01-4835-95d0-1839b46351bb" />

- enabling eve.json in suricata.yaml which enables to send live data to the web dashboard
<img width="1920" height="1080" alt="new enabling JSON eve" src="https://github.com/user-attachments/assets/4a67f3c9-88ba-4359-9ea4-2d6f066c0094" />

- Starting evebox server
<img width="1920" height="1080" alt="new evebox server start" src="https://github.com/user-attachments/assets/ecac9bd6-77c7-46ad-a7fc-dc0cc047f332" />

### step 6 - updating local rules: Suricata.yaml

- adding rules 
<img width="1920" height="1080" alt="typing adding local rules" src="https://github.com/user-attachments/assets/b311f51d-bd8a-46d2-81dc-43d1cee6f4b5" />

- verifying whether the updated rules working by starting suricata
<img width="1920" height="1080" alt="verifying new local rules are added" src="https://github.com/user-attachments/assets/a512a9e7-83cf-4f88-b8ec-bb344be8b06f" />

### Step 6 — Python Script: Live Telegram Alert Notifications

Wrote a **Python script** that continuously monitors the `eve.json` file for new Suricata events. When an attack is detected, the script parses the alert details and sends an instant notification to a configured **Telegram bot**.

- creating a bot which shares eve.json log to a telegram chat (Using a telegram bot)
<img width="1920" height="1080" alt="suricata final telegram bot removed repetition msges removed icons add location" src="https://github.com/user-attachments/assets/ba020cc4-51e9-4d47-9b39-b8d997a705db" />


## 📁 Final test of the SOC lab

- Starting vulnerable services in victim vm
<img width="1920" height="1080" alt="final starting the vulnerable services" src="https://github.com/user-attachments/assets/488f2e36-a657-4851-9512-c95e798cf48e" />

- setting DVWA security level to low
<img width="1920" height="1080" alt="final dvwa low" src="https://github.com/user-attachments/assets/2cc2d2a6-dcb2-4dc8-98b7-7f009de69081" />

### Starting suricata service
<img width="1920" height="1080" alt="final starting suricata serice" src="https://github.com/user-attachments/assets/e7553a43-498b-4af5-ad33-6cd8ee99689d" />

### Starting evebox web dashboard
<img width="1920" height="1080" alt="final running evebox" src="https://github.com/user-attachments/assets/c94b9f0f-5596-42d2-b6b4-f84149566b0a" />

### Running python script which direct eve.json log to telegram bot
<img width="1920" height="1080" alt="running python script" src="https://github.com/user-attachments/assets/c67b69cb-69ec-454b-b862-abe9f044dcc0" />

- Telegram message that inform whether the bot is active
<img width="591" height="1280" alt="telegram bot stating messege" src="https://github.com/user-attachments/assets/054471e0-47b7-4862-98a3-c601647fba88" />










---

## ⚠️ Disclaimer

This lab is built for **educational and ethical purposes only**. All attacks are performed in an isolated virtual environment with no connection to real-world systems or networks.
