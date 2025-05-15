# 🚀 aria2 + AriaNg on Windows (Service + GUI Setup)

This guide walks you through installing **aria2** on Windows, running it as a background service using **NSSM**, and managing your downloads via the AriaNg web interface.

---

## 📦 Requirements

- Windows 10 or 11
- Admin privileges
- Internet connection

---

## 🔧 Step 1: Install aria2

1. **Download aria2**  
   Go to: https://github.com/aria2/aria2/releases  
   Download: `aria2-*-win-64bit-build1.zip`

2. **Extract to a folder**  
   Example: `C:\aria2`

3. **Create a download directory**  
   ```
   C:\aria2\Downloads
   ```

---

## ⚙️ Step 2: Configure aria2

1. Create `aria2.conf` in `C:\aria2` with the following content:

   ```ini
   dir=C:/aria2/Downloads
   continue=true
   file-allocation=prealloc
   enable-rpc=true
   rpc-listen-all=true
   rpc-allow-origin-all=true
   rpc-secret=mySecretToken123
   input-file=aria2.session
   save-session=aria2.session
   save-session-interval=30
   ```

   Replace `mySecretToken123` with a secure password.

2. Create an empty file `aria2.session` in the same folder:
   ```
   C:\aria2\aria2.session
   ```

---

## 🛠 Step 3: Run aria2 as a Windows Service using NSSM

1. **Download NSSM**  
   From: https://nssm.cc/download  
   Extract to: `C:\nssm`

2. **Install aria2 as a service**

   Open Command Prompt as Administrator and run:

   ```bash
   C:\nssm\win64\nssm.exe install aria2
   ```

   Fill in the GUI:

   - **Path**: `C:\aria2\aria2c.exe`
   - **Startup directory**: `C:\aria2`
   - **Arguments**:  
     ```
     --conf-path=aria2.conf
     ```

   Click **Install Service**.

3. **Start the service**:

   ```bash
   net start aria2
   ```

   Or open `services.msc`, find **aria2**, right-click → **Start**.

---

## 🌐 Step 4: Use AriaNg (Graphical Frontend)

### Option 1: Use Web Version

- Open: https://ariang.mayswind.net/latest/

### Option 2: Run AriaNg Locally

1. Download `AriaNg-AllInOne.zip`:  
   https://github.com/mayswind/AriaNg/releases

2. Extract and open `index.html` in your browser.

---

## 🔗 Step 5: Connect AriaNg to aria2

In AriaNg settings (gear icon):

- **Host**: `127.0.0.1`
- **Port**: `6800`
- **Interface**: `JSON-RPC`
- **Secret Token**: `mySecretToken123`

> AriaNg will connect and show active downloads and logs if everything is working correctly.

---

## 🔐 Security Tips

- Do **not expose port 6800** to the internet without protection.
- Always set a **strong `rpc-secret`** in `aria2.conf`.
- If remote access is needed, use:
  - [Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/)
  - VPN / SSH Tunnel
  - HTTPS reverse proxy (e.g., Nginx + Certbot)

---

## ✅ Service Management Commands

```bash
# Start aria2 service
net start aria2

# Stop aria2 service
net stop aria2

# Remove service (optional)
nssm remove aria2 confirm
```

---

## 🧠 Additional Resources

- [aria2 GitHub](https://github.com/aria2/aria2)
- [AriaNg GitHub](https://github.com/mayswind/AriaNg)
- [NSSM (Service Manager)](https://nssm.cc/)

---

## 📌 Credits

- aria2 by Tatsuhiro Tsujikawa
- AriaNg by mayswind
- NSSM by Iain Patterson