
# Daemon Management with Monit and Supervisor

A **daemon** is a background program that provides services or monitors your system. Tools like **Monit**, **Supervisor**, **cron**, or **sshd** all run as daemons.

---

## Monit vs. Supervisor

| Feature            | **Monit**                            | **Supervisor**                         |
| ------------------ | ------------------------------------ | -------------------------------------- |
| **Purpose**        | System monitoring and self-healing   | Process management for apps/scripts    |
| **Best for**       | System services (e.g., Nginx, MySQL) | App processes (e.g., Python, Node.js)  |
| **Monitoring**     | Yes (CPU, memory, file checks, etc.) | No (only checks if process is running) |
| **Auto-Restart**   | ✅ Yes                                | ✅ Yes                                  |
| **Alerting**       | ✅ Built-in email alerts              | ❌ Requires custom scripts              |
| **Web Interface**  | ✅ Yes (via port 2812)                | ❌ No (unless extended with plugins)    |
| **Resource Usage** | Lightweight                          | Lightweight                            |
| **Config Style**   | Custom declarative syntax            | INI file format                         |

---

## Monit Installation & Configuration on Ubuntu

```bash
sudo apt update
sudo apt install monit -y
sudo systemctl enable monit
sudo systemctl status monit
monit -v
```

### Configuration (Example: Monitor NGINX)

Edit Monit config:
```bash
sudo nano /etc/monit/monitrc
```

Add:
```monit
set httpd port 2812 and
    use address 127.0.0.1
    allow admin:monit

check process nginx with pidfile /run/nginx.pid
    start program = "/usr/sbin/service nginx start"
    stop program  = "/usr/sbin/service nginx stop"
    if failed 127.0.0.1 port 80 protocol http for 3 cycles then restart
```

Reload Monit:
```bash
sudo monit -t
sudo monit reload
sudo monit status
```

Check in browser: `http://127.0.0.1:2812`

### Alerting (Email Notifications)
```monit
set mailserver smtp.gmail.com port 587
    username = "your_email@gmail.com"
    password = "your_app_password"
    using tlsv12

set alert your_email@gmail.com

check system localhost
    if loadavg (1min) > 4 then alert
    if memory usage > 75% then alert
    if cpu usage (user) > 70% then alert

check filesystem rootfs with path /
    if space usage > 85% then alert
```

Stop Nginx to trigger alert:
```bash
sudo systemctl stop nginx
```

---

## Supervisor Installation & Configuration

```bash
sudo apt install supervisor -y
sudo systemctl enable supervisor
sudo systemctl start supervisor
supervisord -v
```

### Python App Example
```bash
mkdir ~/pyapp
cd ~/pyapp
nano myapp.py
```
**myapp.py**
```python
import time

while True:
    print("Python app is running...")
    time.sleep(5)
```

### Configure Supervisor for Python App
```bash
sudo nano /etc/supervisor/conf.d/pyapp.conf
```
```ini
[program:pyapp]
command=/usr/bin/python3 /home/ubuntu/pyapp/myapp.py
autostart=true
autorestart=true
stdout_logfile=/var/log/pyapp.out.log
stderr_logfile=/var/log/pyapp.err.log
```

### Node App Example
```bash
sudo nano /etc/supervisor/conf.d/nodeapp.conf
```
```ini
[program:nodeapp]
command=/usr/bin/node /home/ubuntu/nodeapp/index.js
autostart=true
autorestart=true
stdout_logfile=/var/log/nodeapp.out.log
stderr_logfile=/var/log/nodeapp.err.log
```

### Reload & Monitor
```bash
sudo supervisorctl reread
sudo supervisorctl update
sudo supervisorctl status pyapp
sudo supervisorctl status nodeapp
```

---

## Graceful Shutdown in Python (for Supervisor)

```python
import time
import signal
import sys

running = True

def handle_exit(signum, frame):
    global running
    print("\nReceived termination signal. Cleaning up...")
    running = False

signal.signal(signal.SIGINT, handle_exit)
signal.signal(signal.SIGTERM, handle_exit)

print("Python app started.")
while running:
    print("Python app is running...")
    time.sleep(5)

print("Shutdown complete. Exiting.")
```

---

## Basic Event Listener in Python

```python
class EventEmitter:
    def __init__(self):
        self.listeners = {}

    def on(self, event, callback):
        if event not in self.listeners:
            self.listeners[event] = []
        self.listeners[event].append(callback)

    def emit(self, event, *args, **kwargs):
        for callback in self.listeners.get(event, []):
            callback(*args, **kwargs)

# Example usage
emitter = EventEmitter()

def on_shutdown():
    print("Shutting down...")

emitter.on('shutdown', on_shutdown)

emitter.emit('shutdown')
```

---

## Monit + Supervisor + Systemd Comparison

| Feature                | **Systemd**                       | **Supervisor**                   | **Monit**                      |
| ---------------------- | --------------------------------- | -------------------------------- | ------------------------------ |
| Primary Use            | Init system, service management   | Process supervision              | Process & system monitoring    |
| Auto-Restart           | ✅ Yes                             | ✅ Yes                            | ✅ Yes                          |
| Process Monitoring     | 🚫 (basic only)                   | ✅ Good                           | ✅ Advanced                     |
| System Resource Checks | ❌                                 | ❌                                | ✅ (CPU, RAM, disk, etc.)       |
| Email Alerts           | ❌                                 | ❌                                | ✅ Yes                          |
| Web Interface          | ❌                                 | ❌                                | ✅ Yes                          |
| Logs                   | `journalctl`                      | stdout/stderr to log files       | Custom log paths               |
| Configuration Style    | Declarative `.service` files      | INI-style `.conf` files          | Custom `.monitrc` or snippets  |
| Daemon Required        | Yes (part of OS)                  | Yes (supervisord)                | Yes (monit daemon)             |
| Boot Management        | ✅ Excellent (`enable`, `disable`) | ❌ (manual or via cron/systemd)   | ❌ (manual or via Systemd)      |
| Use Case               | OS services, system boot          | Python, Node.js, background apps | Monitoring and auto-healing    |

---

## Auto Start Python App with Monit

```bash
sudo nano /etc/monit/conf-enabled/myapp
```
```monit
check process myapp
    matching "app.py"
    start program = "/usr/bin/python3 /home/ubuntu/myapp/app.py"
    stop program  = "/usr/bin/pkill -f /home/ubuntu/myapp/app.py"
    if not exist then restart
```

---

## Summary
- **Monit** is great for system/service monitoring, auto-healing, and alerting.
- **Supervisor** is perfect for managing long-running app processes.
- **Systemd** is ideal for OS-level services and boot-time management.

Choose tools based on your specific need: Monitoring vs. App Management vs. Boot Service Control.

---

**Happy Monitoring!** 🎯
