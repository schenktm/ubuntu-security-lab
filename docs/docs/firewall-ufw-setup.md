# Firewall setup with UFW

This file documents the basic firewall setup on my Ubuntu server.

---

## Install UFW

### Command

```bash
sudo apt install ufw -y
```

### Command breakdown

| Part      | Meaning                     | German              |
| --------- | --------------------------- | ------------------- |
| `sudo`    | superuser do / run as admin | als Admin ausführen |
| `apt`     | Advanced Package Tool       | Paketverwaltung     |
| `install` | install software            | installieren        |
| `ufw`     | Uncomplicated Firewall      | einfache Firewall   |
| `-y`      | yes automatically           | automatisch ja      |

### What it does

Installs UFW, the simple firewall tool for Ubuntu.

Deutsch: Installiert die einfache Firewall für Ubuntu.

### Result

```text
ufw is already the newest version.
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

---

## Allow SSH

### Command

```bash
sudo ufw allow OpenSSH
```

### Command breakdown

| Part      | Meaning                     | German              |
| --------- | --------------------------- | ------------------- |
| `sudo`    | superuser do / run as admin | als Admin ausführen |
| `ufw`     | Uncomplicated Firewall      | einfache Firewall   |
| `allow`   | allow traffic               | Verkehr erlauben    |
| `OpenSSH` | SSH service profile         | SSH-Zugang          |

### What it does

Allows SSH connections through the firewall.

Deutsch: Die Firewall erlaubt SSH-Verbindungen.

### Why

SSH must be allowed before enabling the firewall, otherwise I could lock myself out.

Deutsch: SSH muss vorher erlaubt sein, sonst kann man sich aussperren.

### Result

```text
Rules updated
Rules updated (v6)
```

---

## Enable firewall

### Command

```bash
sudo ufw enable
```

### Command breakdown

| Part     | Meaning                     | German              |
| -------- | --------------------------- | ------------------- |
| `sudo`   | superuser do / run as admin | als Admin ausführen |
| `ufw`    | Uncomplicated Firewall      | einfache Firewall   |
| `enable` | turn on                     | einschalten         |

### What it does

Turns on the firewall.

Deutsch: Schaltet die Firewall ein.

### Result

```text
Firewall is active and enabled on system startup
```

---

## Check firewall status

### Command

```bash
sudo ufw status verbose
```

### Command breakdown

| Part      | Meaning                     | German              |
| --------- | --------------------------- | ------------------- |
| `sudo`    | superuser do / run as admin | als Admin ausführen |
| `ufw`     | Uncomplicated Firewall      | einfache Firewall   |
| `status`  | show status                 | Status anzeigen     |
| `verbose` | detailed                    | ausführlich         |

### What it does

Shows the detailed firewall status.

Deutsch: Zeigt den ausführlichen Firewall-Status.

### Result

```text
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)

22/tcp (OpenSSH) ALLOW IN Anywhere
22/tcp (OpenSSH (v6)) ALLOW IN Anywhere (v6)
```

### What it means

Incoming traffic is blocked by default. SSH is allowed.

Deutsch: Eingehende Verbindungen sind standardmäßig blockiert. SSH ist erlaubt.

---

## Allow HTTP

### Command

```bash
sudo ufw allow 80/tcp
```

### Command breakdown

| Part     | Meaning                       | German                          |
| -------- | ----------------------------- | ------------------------------- |
| `sudo`   | superuser do / run as admin   | als Admin ausführen             |
| `ufw`    | Uncomplicated Firewall        | einfache Firewall               |
| `allow`  | allow traffic                 | Verkehr erlauben                |
| `80`     | HTTP port                     | Webseiten-Port                  |
| `tcp`    | Transmission Control Protocol | zuverlässiges Netzwerkprotokoll |
| `80/tcp` | HTTP traffic over TCP         | Webseitenverkehr über TCP       |

### What it does

Allows normal web traffic over port 80.

Deutsch: Erlaubt normalen Webseitenverkehr über Port 80.

### Result

```text
Rule added
Rule added (v6)
```

---

## Allow HTTPS

### Command

```bash
sudo ufw allow 443/tcp
```

### Command breakdown

| Part      | Meaning                       | German                             |
| --------- | ----------------------------- | ---------------------------------- |
| `sudo`    | superuser do / run as admin   | als Admin ausführen                |
| `ufw`     | Uncomplicated Firewall        | einfache Firewall                  |
| `allow`   | allow traffic                 | Verkehr erlauben                   |
| `443`     | HTTPS port                    | sicherer Webseiten-Port            |
| `tcp`     | Transmission Control Protocol | zuverlässiges Netzwerkprotokoll    |
| `443/tcp` | HTTPS traffic over TCP        | sicherer Webseitenverkehr über TCP |

### What it does

Allows secure web traffic over port 443.

Deutsch: Erlaubt sicheren Webseitenverkehr über Port 443.

### Result

```text
Rule added
Rule added (v6)
```

---

## Final firewall status

### Command

```bash
sudo ufw status verbose
```

### Result

```text
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)

22/tcp (OpenSSH)      ALLOW IN    Anywhere
80/tcp                ALLOW IN    Anywhere
443/tcp               ALLOW IN    Anywhere
22/tcp (OpenSSH v6)   ALLOW IN    Anywhere (v6)
80/tcp (v6)           ALLOW IN    Anywhere (v6)
443/tcp (v6)          ALLOW IN    Anywhere (v6)
```

### Summary

The firewall is active.
Incoming traffic is blocked by default.
Only SSH, HTTP and HTTPS are allowed.

Deutsch: Die Firewall ist aktiv. Eingehend ist alles blockiert, außer SSH, HTTP und HTTPS.

---

## Check firewall rules with numbers

### Command

```bash
sudo ufw status numbered
```

### Command breakdown

| Part       | Meaning                     | German                      |
| ---------- | --------------------------- | --------------------------- |
| `sudo`     | superuser do / run as admin | als Admin ausführen         |
| `ufw`      | Uncomplicated Firewall      | einfache Firewall           |
| `status`   | show status                 | Status anzeigen             |
| `numbered` | show rule numbers           | Regeln mit Nummern anzeigen |

### What it does

Shows the active firewall rules with rule numbers.

Deutsch: Zeigt die aktiven Firewall-Regeln mit Nummern.

### Result

```text
Status: active

[ 1] OpenSSH      ALLOW IN    Anywhere
[ 2] 80/tcp       ALLOW IN    Anywhere
[ 3] 443/tcp      ALLOW IN    Anywhere
[ 4] OpenSSH (v6) ALLOW IN    Anywhere (v6)
[ 5] 80/tcp (v6)  ALLOW IN    Anywhere (v6)
[ 6] 443/tcp (v6) ALLOW IN    Anywhere (v6)
```

### Note about port 80

Port `80/tcp` is used for normal HTTP web traffic.

Deutsch: Port `80/tcp` ist für normalen Webseitenverkehr.

Port 80 is not automatically a security issue. It is often used for HTTP access and later redirecting HTTP to HTTPS.

Deutsch: Port 80 ist nicht automatisch eine Sicherheitslücke. Er wird oft für HTTP und spätere Weiterleitung auf HTTPS genutzt.

Sensitive data should use HTTPS on port `443/tcp`.

Deutsch: Sensible Daten sollen über HTTPS auf Port `443/tcp` laufen.
