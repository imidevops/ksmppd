
# Ksmppd Service Management (My Contributions)

This document explains the service management scripts I added for **ksmppd** and **Kannel** to run as proper daemons on Linux systems.

---

## 🚀 Overview

- Added **SysV init scripts** for `ksmppd` and `kannel`
- Supports `start`, `stop`, `restart`, and `status` (for Kannel)
- Added **daemonization support** with PID file management
- Can be registered to start automatically on system boot
- Provides both `init.d` and modern `systemd` service unit examples

---

## 📂 Scripts Added

### 🔹 Kannel Init Script
- Location: `/etc/init.d/kannel`
- Functions: `start`, `stop`, `restart`, `status`
- Example usage:
  ```bash
  cd /etc/init.d
  ./kannel start
  ./kannel stop
  ./kannel restart

### 🔹 Ksmppd Init Script

- **Location:** `/etc/init.d/ksmppd`  
- **Functions:** `start`, `stop`, `restart`  

**Example usage:**
```bash
cd /etc/init.d
./ksmppd start
./ksmppd stop
./ksmppd restart

🔄 Auto-Start on Boot  
```
**Debian/Ubuntu**  
```bash
sudo update-rc.d kannel defaults
sudo update-rc.d ksmppd defaults
```

**CentOS/RHEL**  
```bash
sudo chkconfig --add kannel
sudo chkconfig --add ksmppd
```

---

⚡ **Systemd Units (Recommended)**  

For modern Linux systems, I also created systemd units.  

**Example: ksmppd**  

_File: `/etc/systemd/system/ksmppd.service`_  

```ini
[Unit]
Description=Ksmppd SMPP Daemon
After=network.target

[Service]
ExecStart=/usr/local/bin/ksmppd --daemonize -p /var/run/ksmppd.pid /usr/local/src/ksmppd/ksmppd.conf
PIDFile=/var/run/ksmppd.pid
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

**Example: kannel**  

_File: `/etc/systemd/system/kannel.service`_  

```ini
[Unit]
Description=Kannel SMS/WAP Gateway
After=network.target

[Service]
ExecStart=/usr/local/sbin/bearerbox -v 1 /etc/kannel/kannel_server.conf
ExecStartPost=/usr/local/sbin/smsbox /etc/kannel/kannel_server.conf
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

---

✅ **Usage with Systemd**  

```bash
systemctl daemon-reload
systemctl enable ksmppd
systemctl start ksmppd
systemctl status ksmppd

systemctl enable kannel
systemctl start kannel
systemctl status kannel
```

---

📝 **Notes**  
- SysV scripts work across older distros.  
- Systemd units are recommended for production.  
- Tested on Debian 9/10/11 and Ubuntu 18.04/20.04.  
