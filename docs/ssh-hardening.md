# SSH hardening

This file documents the SSH hardening steps on my Ubuntu server.

---

## Backup SSH config

### Command

```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
```

### Command breakdown

| Part                       | Meaning                     | German                   |
| -------------------------- | --------------------------- | ------------------------ |
| `sudo`                     | superuser do / run as admin | als Admin ausführen      |
| `cp`                       | copy                        | kopieren                 |
| `/etc/ssh/sshd_config`     | SSH server config           | SSH-Server-Konfiguration |
| `/etc/ssh/sshd_config.bak` | backup file                 | Sicherungsdatei          |
| `.bak`                     | backup                      | Sicherung                |

### What it does

Creates a backup of the SSH server configuration.

Deutsch: Erstellt eine Sicherung der SSH-Konfiguration.

### Why

Before changing SSH security settings, I want a backup.

Deutsch: Vor Security-Änderungen erst eine Sicherung machen.

---

## Check backup file

### Command

```bash
ls -l /etc/ssh/sshd_config*
```

### Command breakdown

| Part                    | Meaning          | German             |
| ----------------------- | ---------------- | ------------------ |
| `ls`                    | list             | anzeigen           |
| `-l`                    | long format      | Details            |
| `/etc/ssh/sshd_config*` | SSH config files | SSH-Konfig-Dateien |
| `*`                     | wildcard         | Platzhalter        |

### Result

```text
-rw-r--r-- 1 root root 3517 ... /etc/ssh/sshd_config
-rw-r--r-- 1 root root 3517 ... /etc/ssh/sshd_config.bak
```

### What it means

The original SSH config and the backup file both exist.

Deutsch: Die originale Datei und die Backup-Datei sind vorhanden.

---

## Create SSH hardening config

### Command

```bash
sudo nano /etc/ssh/sshd_config.d/99-hardening.conf
```

### Command breakdown

| Part                      | Meaning                     | German              |
| ------------------------- | --------------------------- | ------------------- |
| `sudo`                    | superuser do / run as admin | als Admin ausführen |
| `nano`                    | text editor                 | Texteditor          |
| `/etc/ssh/sshd_config.d/` | SSH config folder           | SSH-Konfig-Ordner   |
| `99`                      | load late                   | spät laden          |
| `hardening`               | security strengthening      | Absicherung         |
| `.conf`                   | config file                 | Konfig-Datei        |

### What it does

Creates a separate SSH hardening config file.

Deutsch: Erstellt eine eigene SSH-Sicherheitsdatei.

### Why

I keep the original SSH config clean and place my own security rules in a separate file.

Deutsch: Die Originaldatei bleibt sauber. Die Sicherheitsregeln kommen in eine eigene Datei.

---

## SSH hardening rules

### Config

```text
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
KbdInteractiveAuthentication no
MaxAuthTries 3
X11Forwarding no
AllowAgentForwarding no
ClientAliveInterval 300
ClientAliveCountMax 2
LogLevel VERBOSE
```

### Rule breakdown

| Rule                              | Meaning                                  | German                          |
| --------------------------------- | ---------------------------------------- | ------------------------------- |
| `PermitRootLogin no`              | block direct root SSH login              | root-SSH sperren                |
| `PubkeyAuthentication yes`        | allow SSH key login                      | SSH-Key erlauben                |
| `PasswordAuthentication no`       | block password SSH login                 | Passwort-SSH sperren            |
| `KbdInteractiveAuthentication no` | block interactive password login         | interaktive Passwortabfrage aus |
| `MaxAuthTries 3`                  | max 3 login attempts                     | maximal 3 Versuche              |
| `X11Forwarding no`                | disable GUI forwarding                   | Grafikweiterleitung aus         |
| `AllowAgentForwarding no`         | disable SSH agent forwarding             | Agent-Weiterleitung aus         |
| `ClientAliveInterval 300`         | check inactive clients after 300 seconds | Inaktivität prüfen              |
| `ClientAliveCountMax 2`           | disconnect after 2 missed checks         | nach 2 Checks trennen           |
| `LogLevel VERBOSE`                | more detailed SSH logs                   | mehr SSH-Logs                   |

### What it does

Hardens SSH access and reduces attack surface.

Deutsch: SSH wird abgesichert und die Angriffsfläche wird kleiner.

---

## Test SSH config

### Command

```bash
sudo sshd -t
```

### Command breakdown

| Part   | Meaning                     | German               |
| ------ | --------------------------- | -------------------- |
| `sudo` | superuser do / run as admin | als Admin ausführen  |
| `sshd` | SSH daemon / SSH server     | SSH-Server-Dienst    |
| `-t`   | test config                 | Konfiguration testen |

### What it does

Tests the SSH server configuration for errors.

Deutsch: Prüft die SSH-Konfiguration auf Fehler.

### Result

```text
No output
```

### What it means

No output means the SSH configuration test passed.

Deutsch: Keine Ausgabe bedeutet, dass die SSH-Konfiguration gültig ist.

---

## Reload SSH service

### Command

```bash
sudo systemctl reload ssh
```

### Command breakdown

| Part        | Meaning                     | German                  |
| ----------- | --------------------------- | ----------------------- |
| `sudo`      | superuser do / run as admin | als Admin ausführen     |
| `systemctl` | control system services     | Systemdienste steuern   |
| `reload`    | reload config               | Konfiguration neu laden |
| `ssh`       | SSH service                 | SSH-Dienst              |

### What it does

Reloads the SSH service so the new rules are applied.

Deutsch: Lädt den SSH-Dienst neu, damit die neuen Regeln aktiv werden.

---

## Test normal user SSH login

### Command

```bash
ssh tim@[server-ip]
```

### What it does

Tests if the normal user `tim` can still log in with SSH.

Deutsch: Prüft, ob `tim` sich weiterhin per SSH anmelden kann.

### Result

```text
tim login works
```

### What it means

The SSH hardening did not lock out the normal admin user.

Deutsch: Der normale Admin-Benutzer wurde nicht ausgesperrt.

---

## Test blocked root SSH login

### Command

```bash
ssh root@[server-ip]
```

### Result

```text
Permission denied (publickey).
```

### What it means

Direct root SSH login is blocked.

Deutsch: Direkter SSH-Login als `root` ist gesperrt.

---

## Check SSH service

### Command

```bash
sudo systemctl status ssh --no-pager
```

### Command breakdown

| Part         | Meaning                     | German                |
| ------------ | --------------------------- | --------------------- |
| `sudo`       | superuser do / run as admin | als Admin ausführen   |
| `systemctl`  | control system services     | Systemdienste steuern |
| `status`     | show status                 | Status anzeigen       |
| `ssh`        | SSH service                 | SSH-Dienst            |
| `--no-pager` | show directly               | direkt anzeigen       |

### What it does

Checks if the SSH service is running.

Deutsch: Prüft, ob der SSH-Dienst läuft.

### Result

```text
sshd: /usr/sbin/sshd -D [listener]
```

### What it means

The SSH service is running and listening for connections.

Deutsch: Der SSH-Dienst läuft und wartet auf Verbindungen.

---

## Check SSH listening port

### Command

```bash
sudo ss -tulpen | grep ':22'
```

### Command breakdown

| Part         | Meaning                     | German                        |
| ------------ | --------------------------- | ----------------------------- |
| `sudo`       | superuser do / run as admin | als Admin ausführen           |
| `ss`         | socket statistics           | Netzwerkverbindungen anzeigen |
| `-t`         | TCP                         | TCP                           |
| `-u`         | UDP                         | UDP                           |
| `-l`         | listening                   | lauschende Dienste            |
| `-p`         | process                     | Prozess anzeigen              |
| `-e`         | extended info               | Extra-Infos                   |
| `-n`         | numeric ports               | Portnummern anzeigen          |
| `grep ':22'` | filter port 22              | nur Port 22 anzeigen          |

### Result

```text
tcp LISTEN ... 0.0.0.0:22 ... users:(("sshd",...))
tcp LISTEN ... [::]:22 ... users:(("sshd",...))
```

### What it means

SSH is listening on port `22` for IPv4 and IPv6.

Deutsch: SSH lauscht auf Port `22` für IPv4 und IPv6.

---

## Summary

SSH hardening is active.

* `tim` can log in with SSH
* direct `root` SSH login is blocked
* password SSH login is disabled
* SSH key login is enabled
* SSH config test passed
* SSH service is running
* port `22` is listening

Deutsch: SSH ist abgesichert und funktioniert weiterhin für den normalen Admin-Benutzer.
