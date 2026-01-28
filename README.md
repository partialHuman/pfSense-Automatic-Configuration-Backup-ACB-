# 🔐 pfSense Automatic Configuration Backup (ACB)

## 📌 Overview
Automatic Configuration Backup (ACB) in pfSense securely stores firewall configurations in the cloud. It automatically saves system settings whenever changes are made, allowing quick restoration during failures or misconfigurations.

This ensures high availability, disaster recovery, and configuration version control.


---

## 🛠️ Step-by-Step Implementation

---

### ✅ Step 1: Login to pfSense

Open browser and go to:

```
https://<pfSense-IP-address>
```

Login with admin credentials.

---

### ✅ Step 2: Install Cron in pfsense

Go to:

```
System > Package Manger > Intsall packages > cron
```

---

### ✅ Step 3: Open Shell in pfsense

<img width="975" height="581" alt="image" src="https://github.com/user-attachments/assets/c75fb6fa-8ed0-462e-8ce0-ebfa068edf6c" />

---

### ✅ Step 4: Install rclone

- In shell type this command to download rclone:

```
fetch https://downloads.rclone.org/rclone-current-freebsd-amd64.zip
```

- Unzip the downloaded file:
```
unzip rclone-current-freebsd-amd64.zip
```

- Move the binary to /usr/local/bin/ so it can be used system-wide:
```
mv rclone-*/rclone /usr/local/bin/
chmod +x /usr/local/bin/rclone
```

- Verify installation:
```
rclone version
```


---

### ✅ Step 5: Google drive setup

```
- Navigate to google cloud console:
```
<img width="975" height="454" alt="image" src="https://github.com/user-attachments/assets/21505124-a9ea-4710-8596-0c4a007df619" />

```
- Go to pojects > new project
- Create a new project
- Then in the navigation menu go to APIs and service > OAuth consent screen
```

<img width="893" height="433" alt="image" src="https://github.com/user-attachments/assets/e539a495-9dfa-4831-abc8-5c6bdb237d53" />

```
- Go to audience > + ADD USERS
- Now navigate to api’s and services  > credentials
```

<img width="891" height="455" alt="image" src="https://github.com/user-attachments/assets/92bd69de-4cb9-4c0a-ac00-051564ef3591" />

```
- Copy your client id and client secret
```


---

### ✅ Step 6: Configure rclone for Google Drive


- Run the following command to start rclone setup:
```
rclone config
```
- Type n and press Enter to create a new remote.
- Enter a name for the remote (e.g., gdrive).
- Choose the storage type: Type 20 (Google Drive) and press Enter.
- Client ID & Client Secret: paste it here
- Scope selection: Type 1 (Full access) and press Enter.
- Root Folder ID: Press Enter to leave blank.
- Service Account: Press Enter to leave blank.
- Advanced Config: Type n and press Enter.
- Auto Config?
   - Type n and press Enter.
   - A URL will appear copy it

<img width="974" height="1048" alt="image" src="https://github.com/user-attachments/assets/33e81557-cb65-41e5-a3c5-4f90b3fa5ab8" />

---

### ✅ Step 6: Config token 

- Download rclone from (rclone.org/downloads) in  your host machine
- A zip file will download. Unzip the file.
- Inside the folder type cmd in the folder’s path
- Command prompt will open paste the url in it
- It will automatically open the sign in page in the browser
- Login in into you account (you will get a success message)
- In the command prompt you will find the client token. Copy it


Scroll down to:

```
Backup History
```

Shows:

- Date & time  
- Configuration versions  

---

### ✅ Step 7: Restore Configuration (Recovery Mode)

Go to:

```
System → Configuration → Restore
```

Select backup version  
Click:

🔄 Restore Configuration

Firewall will reboot and restore settings.

---



---

## 📜 License

MIT License  
