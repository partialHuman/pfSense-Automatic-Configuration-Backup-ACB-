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


- Navigate to google cloud console:
- Go to pojects > new project
- Create a new project
- Then in the navigation menu go to APIs and service > OAuth consent screen
- Go to audience > + ADD USERS
- Now navigate to api’s and services  > credentials
- Copy your client id and client secret


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
   - A URL will appear *copy it*


---

### ✅ Step 7: Config token 

- Download rclone from [rclone](https://rclone.org/downloads/) in  your **Host Machine**
- A zip file will download. Unzip the file.
- Inside the folder type *cmd* in the folder’s path
- Command prompt will open paste the url in it
- It will automatically open the sign in page in the browser
- Login in into you account (you will get a success message)
- In the command prompt you will find the client token. *Copy it*
- Paste the token in the pfsense console
- After setup, list your remotes to confirm:
```
rclone listremotes
```
-You should see:
```
gdrive:
```


### ✅ Step 8: Create the Backup Script in /root/

Navigate to root directory:

```
cd /root/
```

Create and edit the script using nano:
```
nano /root/backup_pfsense.sh
```

Copy and paste the script inside the file:

```
#!/bin/sh 
# Set backup directory and filename
BACKUP_DIR="/root/pfsense_backups"
BACKUP_FILE="config-$(date +%F-%H%M).xml"
RCLONE_REMOTE="gdrive:pfsense_backups"
 # Create backup directory if it doesn't exist
mkdir -p "$BACKUP_DIR"
# Copy pfSense configuration
cp /cf/conf/config.xml "$BACKUP_DIR/$BACKUP_FILE"
# Upload backup to Google Drive
rclone copy "$BACKUP_DIR/$BACKUP_FILE" "$RCLONE_REMOTE"
# Delete local backups older than 7 days
find "$BACKUP_DIR" -type f -name "config-*.xml" -mtime +7 -exec rm {} \;
echo "Backup completed and uploaded to Google Drive."

```

Save and exit:
```
Press CTRL + X, then Y, then Enter.
```
Make the Script Executable
```
chmod +x /root/backup_pfsense.sh
```

Test the Script
```
/root/backup_pfsense.sh
```


🔄 Check if the backup is uploaded to Google Drive.

---

### ✅ Step 9: Automate the Backup with Cron

Open the cron job editor:
```
crontab -e
```

Add this line at the end to run the backup every night at midnight:
```
0 0 * * * /root/backup_pfsense.sh >> /root/backup.log 2>&1
```

Save and exit.
```
Press Esc then :wq to save and exit
```

Verify the Scheduled Job
```
crontab -l
You should see:
0 0 * * * /root/backup_pfsense.sh >> /root/backup.log 2>&1
```

*Now, pfSense will automatically back up the configuration to Google Drive every night at 12:00 AM.*

---

## 📜 License

MIT License  
