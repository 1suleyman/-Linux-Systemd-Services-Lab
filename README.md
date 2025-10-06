# 🐧 Linux Systemd Services Lab

In this lab, I learned how to **inspect, manage, and configure systemd services** on a Linux system. This included starting, stopping, enabling, and editing services, as well as troubleshooting issues with service definitions.

---

## 📋 Lab Overview

**Goal:**

* Inspect the status of services
* Start, stop, and restart systemd services
* Enable services to start automatically on boot
* Edit service unit files to specify execution commands and restart policies
* Troubleshoot service failures using system logs

**Learning Outcomes:**

* Use `systemctl` to manage services (`start`, `stop`, `status`, `enable`)
* Modify service unit files using `systemctl edit` or direct editing in `/etc/systemd/system`
* Apply restart policies using `Restart=` in service units
* Inspect errors using `journalctl`
* Understand service dependencies and permissions

---

## 🛠 Step-by-Step Journey

### Step 1: Check Service Status

**Task:** Check the status of `sample.service`.

```bash
sudo systemctl status sample.service
```

* Output may show `inactive (dead)` if the service is not running.

---

### Step 2: Start the Service

**Task:** Attempt to start `sample.service`.

```bash
sudo systemctl start sample.service
```

* If it fails, check the error message.
* Common error: `org.freedesktop.policykit1 was not provided by any .service files`.

---

### Step 3: Edit the Service Unit

**Task:** Define the executable script for the service.

```bash
sudo systemctl edit sample.service --full
```

* In the `[Service]` section, add:

```
ExecStart=/bin/bash /root/sample_script.sh
```

* Save and exit (`ESC`, then `:wq!`).

---

### Step 4: Reload Systemd Configuration

```bash
sudo systemctl daemon-reload
```

* Reloads all unit files to apply configuration changes.

---

### Step 5: Start and Verify Service

```bash
sudo systemctl start sample.service
sudo systemctl status sample.service
```

* Ensure the service is **active (running)**.

---

### Step 6: Enable Service on Boot

**Task:** Start automatically after reboot.

```bash
sudo systemctl enable sample.service
```

* Creates a symlink in `/etc/systemd/system/multi-user.target.wants/`.

---

### Step 7: Configure Automatic Restart

**Task:** Ensure service restarts if it stops.

```bash
sudo systemctl edit sample.service --full
```

* Add in `[Service]` section:

```
Restart=always
```

* Save, exit, and reload:

```bash
sudo systemctl daemon-reload
sudo systemctl restart sample.service
```

---

### Step 8: Troubleshoot Service Errors

**Task:** View logs of the service to diagnose issues.

```bash
sudo journalctl -u sample.service
```

* Helpful if a service fails due to missing `ExecStart` or other configuration errors.

---

## ✅ Key Commands Summary

| Task                   | Command / Notes                             |
| ---------------------- | ------------------------------------------- |
| Check status           | `sudo systemctl status sample.service`      |
| Start service          | `sudo systemctl start sample.service`       |
| Enable service on boot | `sudo systemctl enable sample.service`      |
| Edit unit file         | `sudo systemctl edit sample.service --full` |
| Reload daemon          | `sudo systemctl daemon-reload`              |
| Restart service        | `sudo systemctl restart sample.service`     |
| View logs              | `sudo journalctl -u sample.service`         |

---

## 💡 Notes / Tips

* Always use **sudo** for managing system services.
* Use `ExecStart=` in the `[Service]` section to specify which script or binary the service runs.
* Apply `Restart=` policies to ensure services automatically recover from failures.
* `journalctl -u <service>` is invaluable for debugging service issues.
* Editing service units can be done either via `systemctl edit` or directly in `/etc/systemd/system/`.

---

## 📌 Lab Summary

| Step                          | Status | Key Takeaways                                      |
| ----------------------------- | ------ | -------------------------------------------------- |
| Check sample.service status   | ✅      | Found inactive/dead                                |
| Start service                 | ✅      | Initial start failed due to missing ExecStart      |
| Edit service unit             | ✅      | Added `ExecStart=/bin/bash /root/sample_script.sh` |
| Reload systemd                | ✅      | `daemon-reload` applied changes                    |
| Start service successfully    | ✅      | Service active and running                         |
| Enable on boot                | ✅      | `systemctl enable sample.service`                  |
| Add restart policy            | ✅      | `Restart=always`                                   |
| View logs for troubleshooting | ✅      | Used `journalctl -u sample.service`                |

---

## ✅ References

* [Systemd Service Units – Linux Documentation](https://www.freedesktop.org/wiki/Software/systemd/)
* [Managing Services with Systemctl](https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units)
* [Journalctl Command Guide](https://www.cyberciti.biz/faq/linux-unix-bsd-systemctl-journalctl-view-logs/)
