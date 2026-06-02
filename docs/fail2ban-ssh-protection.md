# Fail2Ban SSH protection

This file documents the basic Fail2Ban setup for SSH protection.

---

## Install Fail2Ban

### Command

```bash
sudo apt install fail2ban -y
```

### Command breakdown

| Part       | Meaning                      | German               |
| ---------- | ---------------------------- | -------------------- |
| `sudo`     | superuser do / run as admin  | als Admin ausführen  |
| `apt`      | Advanced Package Tool        | Paketverwaltung      |
| `install`  | install software             | installieren         |
| `fail2ban` | block failed login attackers | Fehlversuche sperren |
| `-y`       | yes automatically            | automatisch ja       |

### What it does

Installs Fail2Ban on the server.

Deutsch: Installiert Fail2Ban auf dem Server.

### Why

Fail2Ban helps protect SSH by blocking IP addresses after too many failed login attempts.

Deutsch: Fail2Ban schützt SSH, indem es IPs nach zu vielen Fehlversuchen sperrt.

### Result

```text
Running kernel seems to be up-to-date.
No services need to be restarted.
```

---

## Check Fail2Ban service

### Command

```bash
sudo systemctl status fail2ban
```

### Command breakdown

| Part        | Meaning                     | German                |
| ----------- | --------------------------- | --------------------- |
| `sudo`      | superuser do / run as admin | als Admin ausführen   |
| `systemctl` | control system services     | Systemdienste steuern |
| `status`    | show status                 | Status anzeigen       |
| `fail2ban`  | Fail2Ban service            | Fail2Ban-Dienst       |

### What it does

Checks if the Fail2Ban service is running.

Deutsch: Prüft, ob der Fail2Ban-Dienst läuft.

### Result

```text
Active: active (running) since Tue 2026-06-02 11:39:05 CEST
```

---

## Check Fail2Ban jails

### Command

```bash
sudo fail2ban-client status
```

### Command breakdown

| Part              | Meaning                     | German              |
| ----------------- | --------------------------- | ------------------- |
| `sudo`            | superuser do / run as admin | als Admin ausführen |
| `fail2ban-client` | Fail2Ban control tool       | Fail2Ban-Werkzeug   |
| `status`          | show status                 | Status anzeigen     |

### What it does

Shows the active Fail2Ban jails.

Deutsch: Zeigt die aktiven Fail2Ban-Schutzregeln.

### Result

```text
Status
|- Number of jail:      1
`- Jail list:   sshd
```

### What it means

There is one active jail, and it protects SSH.

Deutsch: Es gibt eine aktive Schutzregel, und sie schützt SSH.

---

## Check SSH jail details

### Command

```bash
sudo fail2ban-client status sshd
```

### Command breakdown

| Part              | Meaning                     | German              |
| ----------------- | --------------------------- | ------------------- |
| `sudo`            | superuser do / run as admin | als Admin ausführen |
| `fail2ban-client` | Fail2Ban control tool       | Fail2Ban-Werkzeug   |
| `status`          | show status                 | Status anzeigen     |
| `sshd`            | SSH daemon jail             | SSH-Schutzregel     |

### What it does

Shows detailed information about the SSH protection rule.

Deutsch: Zeigt Details zur SSH-Schutzregel.

### Result

```text
Status for the jail: sshd
|- Filter
|  |- Currently failed: 1
|  |- Total failed:     36
|  `- Journal matches:  _SYSTEMD_UNIT=sshd.service + _COMM=sshd
`- Actions
   |- Currently banned: 1
   |- Total banned:     1
   `- Banned IP list:   [155.***.**.**]
```

### What it means

Fail2Ban is active and has already banned one IP address.

Deutsch: Fail2Ban läuft aktiv und hat bereits eine IP-Adresse gesperrt.

The real banned IP is not published here.

Deutsch: Die echte gesperrte IP wird hier nicht veröffentlicht.
