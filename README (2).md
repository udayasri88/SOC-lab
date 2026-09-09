# Splunk SOC Home Lab

A hands-on home lab project for building a mini Security Operations Center (SOC) using **Splunk Enterprise** and the **Splunk Universal Forwarder**, with log collection from Windows endpoints, custom detections, alerting, and a SOC dashboard.

This repo documents the full build process — setup, configuration, commands used, and screenshots/screen recordings of each stage.

---

## 📌 Project Overview

The goal of this lab is to simulate a small-scale SOC environment:

- Set up Splunk Enterprise on Kali Linux as the central log collection and analysis server.
- Deploy the Splunk Universal Forwarder on a Windows machine to ship logs.
- Investigate collected events and build detections using SPL (Search Processing Language).
- Configure alerts for suspicious activity.
- Build a SOC dashboard for monitoring.
- Add basic network monitoring.
- Document the entire process for reference and portfolio purposes.

---

## ✅ Progress Tracker

| Stage                     
|---------------------------
| Lab Setup                 
| Splunk Installation       
| Receiving Configuration   
| Universal Forwarder       
| Windows Log Collection    
| Event Investigation       
| SPL Detections            
| Alerts                    
| SOC Dashboard             
| Network Monitoring        
| Final Documentation       

---

## 🖥️ Lab Environment

- **Splunk Enterprise Server:** Kali Linux (64-bit)
- **Log Source:** Windows machine running Splunk Universal Forwarder
- **Splunk Version:** 10.4.3
- **Network:** Local VM network (forwarder → indexer over port 9997)
- **Splunk Web UI:** `http://127.0.0.1:8000`

---

## ⚙️ Setup & Commands

### 1. Kali Linux — System Checks

```bash
uname -m                   # check CPU architecture (64-bit or 32-bit)
cat /etc/os-release        # check OS version
free -h                    # check available RAM
df -h                      # check available disk space
ls -lh ~/Downloads/*.tgz   # confirm the Splunk installer was downloaded
```

### 2. Verify Download Integrity

```bash
sha512sum splunk-10.4.3-4174a2deda5d-linux-amd64.tgz
```
Generates a SHA-512 hash of the downloaded installer to confirm it hasn't been tampered with or corrupted.

### 3. Install & Start Splunk

```bash
/opt/splunk/bin        # sensitive location where Splunk is installed
./splunk start          # start Splunk (./ = run from current directory)
./splunk status         # check whether Splunk is running
```

Once running, access the Web UI at:
```
http://127.0.0.1:8000
```

### 4. Windows — Universal Forwarder Setup

```powershell
cd "C:\Program Files\SplunkUniversalForwarder\bin"

sc query SplunkForwarder          # check forwarder service status
net start SplunkForwarder         # start the forwarder
net stop SplunkForwarder          # stop the forwarder
splunk restart                    # restart the forwarder

Test-NetConnection 192.168.56.4 -Port 9997   # test connectivity to the Splunk indexer
```

### 5. Configure Forwarding

```powershell
splunk list forward-server
# confirms whether the forward server is correctly configured

splunk add forward-server 192.168.56.4:9997 -auth splunkuser:<PASSWORD>
# sends logs from this Windows machine to the Splunk Enterprise instance
```

### 6. Configure What Gets Forwarded

```powershell
cd "C:\Program Files\SplunkUniversalForwarder\etc\system\local"
notepad inputs.conf
```
`inputs.conf` defines exactly which Windows event logs and data sources get shipped to Splunk.

---

## 📸 Screenshots & Screen Recordings

> Add your media here as the project progresses.

```
/screenshots/
  ├── 01-lab-setup.png
  ├── 02-splunk-install.png
  ├── 03-forwarder-config.png
  └── ...

/recordings/
  └── setup-walkthrough.mp4
```


---

## 📄 Notes

- Passwords and credentials used in commands above are redacted (`<PASSWORD>`) — never commit real credentials to this repo.
- This is a personal learning lab intended for practicing SOC analyst / detection engineering skills.
