```markdown
# Camera Access Exploit Lab  
**Red Team Simulation | Educational Use Only**  
*“See how easy it is to be compromised — then learn how to stop it.”*  

---

## **WARNING: LEGAL & ETHICAL USE ONLY**

> **This tool is for authorized security testing and education.**  
> **Unauthorized use is illegal.**  
> **You must have explicit permission from the device owner.**  

---

## **What This Lab Teaches**

| **Concept** | **Why It Matters** |
|------------|-------------------|
| **HTTPS is mandatory** | HTTP = camera blocked on modern browsers |
| **User gesture required** | No click = no access |
| **Base64 padding & encoding** | One `=` missing = exploit fails |
| **Auto-click "Allow"** | Social engineering + timing = silent access |
| **ngrok tunneling** | Localhost → Public URL in seconds |
| **Self-signed certs** | Trust warnings = first line of defense |
```
---

## **How the Exploit Works (Step-by-Step)**

```mermaid
graph TD
    A[Victim clicks link] --> B[HTTPS page loads]
    B --> C[Button: "Claim Prize"]
    C --> D[JS triggers getUserMedia()]
    D --> E[Permission popup appears]
    E --> F[Auto-click "Allow" in 300ms]
    F --> G[Camera snaps photo]
    G --> H[Base64 image POSTed to server]
    H --> I[Photo saved as JPG]
    I --> J[Redirect to google.com]
```

---

## **Setup (macOS)**

### 1. **Install Dependencies**
```bash
brew install python openssl qrencode
pip3 install --user requests
```

### 2. **Clone & Run**
```bash
git clone https://github.com/yourname/camera-exploit-lab.git
cd camera-exploit-lab
python3 final_armor.py
```

### 3. **Expose with ngrok**
```bash
ngrok http https://localhost:8443
```
> Copy the `https://*.ngrok.io` URL

---

## **Demo Flow (For Students)**

1. **Share QR code** of ngrok URL  
2. **Victim scans & opens**  
3. **Taps "Claim Now"**  
4. **Popup auto-allowed**  
5. **Photo saved in `ARMOR_LOOT/`**  
6. **Victim sees Google.com**  

**Result:** Silent camera access in <3 seconds.

---

## **Defense Lessons (Teach This!)**

| **Attack** | **Defense** |
|----------|-----------|
| Fake prize page | Never allow camera on unknown sites |
| Auto-click "Allow" | **Disable JavaScript** for untrusted sites |
| ngrok public URL | **Block ngrok.io** in firewall/DNS |
| Base64 exfil | **Inspect network traffic** (DevTools) |
| Self-signed cert | **Never click "Proceed"** on warnings |

---

## **Bonus: Block This Exploit**

### **Browser Settings**
```text
Chrome → Settings → Privacy → Camera → Block *.ngrok.io
```

### **Network-Level Block**
```bash
# Pi-hole / AdGuard
||*.ngrok.io^$important
```

### **Endpoint Detection**
```python
# Look for: toDataURL + fetch + POST + img=
if "toDataURL" in js and "fetch" in js and "img=" in js:
    alert("Potential camera exfil!")
```

---

## **Files in This Repo**

```
final_armor.py        ← Working exploit (HTTPS + auto-allow)
server.pem            ← Auto-generated cert
ARMOR_LOOT/           ← Stolen photos
README.md             ← This file
qr.png                ← Demo QR (generate with qrencode)
```

---

## **For Instructors**

1. **Run server**  
2. **Generate QR** → `qrencode -o qr.png "https://abc123.ngrok.io"`  
3. **Project QR in class**  
4. **Ask students to scan**  
5. **Show photo in loot folder**  
6. **Then teach defense**

---

## **Final Words**

> **"The best way to teach security is to show how fragile it is."**  

**Use this lab to wake people up — not to harm them.**

---

**Made with ❤️ & 💀 by Lalith Sagar J**  
**Red Team | Blue Team | Awareness Team**

---
```
