---
title: Setting up Cloudflare DDNS on Ubuntu for Dynamic Home IP
date: 2026-03-20
draft: false
tags:
  - cloudflare
  - ddns
  - networking
  - homelab
  - ubuntu
---
---
title: Setting up Cloudflare DDNS on Ubuntu using cloudflare-ddns-updater
date: 2026-03-20
draft: false
tags:
  - cloudflare
  - ddns
  - homelab
  - ubuntu
  - automation
---

## 🧩 Problem

My home network uses a dynamic public IP address, which changes periodically. This breaks remote access to my self-hosted services.

---

## 🛠️ Solution Overview

I used an open-source tool (cloudflare-ddns-updater) to automatically update my Cloudflare DNS records whenever my public IP changes.

---

## 🔧 Environment

- Ubuntu Server (DDNS host)
- Cloudflare domain
- API Token with DNS edit permissions
- Git installed

---

## 🚀 Step 1: Create Cloudflare API Token

1. Log in to Cloudflare  
2. Go to **My Profile → API Tokens**  
3. Create a token with:
   - Zone → DNS → Edit  
   - Zone Resources → Specific Zone (your domain)

Save the token securely.

---

## 🌐 Step 2: Get Required IDs

You need:
- Zone ID  
- DNS Record ID  

You can find these from:
- Cloudflare dashboard  
- Or via API calls  

---

## 📦 Step 3: Clone the Repository

```bash
sudo apt update
sudo apt install git -y

git clone https://github.com/K0p1-Git/cloudflare-ddns-updater.git
cd cloudflare-ddns-updater
```

---

## ⚙️ Step 4: Configure the Script

Edit the configuration file:

```bash
nano cloudflare-template.sh
```

Update the following values:

```env
CF_API_TOKEN=your_api_token
CF_ZONE_ID=your_zone_id
CF_RECORD_ID=your_record_id
CF_RECORD_NAME=home.yourdomain.com
```

---

## 🧪 Step 5: Run the Script Manually

```bash
bash update.sh
```

This will:
- Check your current public IP
- Compare with Cloudflare DNS
- Update if there is a change

---

## ⏱️ Step 6: Automate with Cron

Edit crontab:

```bash
crontab -e
```

Add:

```bash
*/1 * * * * cd /home/youruser/cloudflare-ddns-updater && bash update.sh >> ddns.log 2>&1
```

---

## 🔍 Step 7: Verify Updates

Check logs:

```bash
cat ddns.log
```

Or verify DNS:

```bash
dig home.yourdomain.com
```

---

## 📊 Result

- DNS automatically updates when IP changes  
- Stable domain access to homelab services  
- Fully automated with minimal overhead  

---

## ⚠️ Challenges Faced

- Incorrect API token permissions  
- Wrong Zone ID / Record ID  
- Script path issues in cron  
- Logs not generating initially  

---

## 🧠 What I Learned

- Practical use of APIs for automation  
- How DNS updates work behind the scenes  
- Importance of logging in automation scripts  

---

## 🚀 Future Improvements

- Use Cloudflare Tunnel (no port forwarding required)  
- Add alerting (email/Discord) on IP change  
- Run script as a systemd service instead of cron  