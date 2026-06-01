# User and SSH setup

This file documents the first user and SSH setup on my Ubuntu server.

---

## How to create a user

### Command

```bash
adduser tim
```

### Command breakdown

| Part      | Meaning  | German              |
| --------- | -------- | ------------------- |
| `add`     | add      | hinzufügen          |
| `user`    | user     | Benutzer            |
| `adduser` | add user | Benutzer hinzufügen |
| `tim`     | username | Benutzername        |

### What it does

Creates a new Linux user called `tim`.

Deutsch: Erstellt den Benutzer `tim`.

### Why

I use a normal user instead of working as `root` all the time.

Deutsch: Sicherer als dauerhaft mit `root` zu arbeiten.

---

## Add sudo rights

### Command

```bash
usermod -aG sudo tim
```

### Command breakdown

| Part      | Meaning                           | German          |
| --------- | --------------------------------- | --------------- |
| `user`    | user                              | Benutzer        |
| `mod`     | modify                            | ändern          |
| `usermod` | modify user                       | Benutzer ändern |
| `-a`      | append                            | hinzufügen      |
| `-G`      | groups                            | Gruppen         |
| `sudo`    | superuser do / admin rights group | Adminrechte     |
| `tim`     | username                          | Benutzername    |

### What it does

Adds the user `tim` to the `sudo` group.

Deutsch: `tim` bekommt Adminrechte über die Gruppe `sudo`.

### Why

I do not want to stay logged in as `root`.
With `sudo`, I can run admin commands only when needed.

Deutsch: Nicht dauerhaft als `root` arbeiten. Mit `sudo` nur kurz Adminrechte benutzen.

---

## Check user groups

### Command

```bash
groups tim
```

### Command breakdown

| Part     | Meaning          | German           |
| -------- | ---------------- | ---------------- |
| `groups` | show user groups | Gruppen anzeigen |
| `tim`    | username         | Benutzername     |

### What it does

Shows which groups the user `tim` belongs to.

Deutsch: Zeigt, in welchen Gruppen `tim` ist.

### Result

```text
tim : tim sudo users
```

### What the result means

| Part    | Meaning            | German                 |
| ------- | ------------------ | ---------------------- |
| `tim`   | own user group     | eigene Benutzergruppe  |
| `sudo`  | admin rights group | Adminrechte            |
| `users` | normal users group | normale Benutzergruppe |

---

## Switch to the new user

### Command

```bash
su - tim
```

### Command breakdown

| Part  | Meaning         | German                |
| ----- | --------------- | --------------------- |
| `su`  | substitute user | Benutzer wechseln     |
| `-`   | login shell     | vollständige Umgebung |
| `tim` | target user     | Zielbenutzer          |

### What it does

Switches from the current user to `tim`.

Deutsch: Wechselt zum Benutzer `tim`.

---

## Check current user

### Command

```bash
whoami
```

### Command breakdown

| Part     | Meaning           | German      |
| -------- | ----------------- | ----------- |
| `whoami` | show current user | wer bin ich |

### What it does

Shows which user is currently active.

Deutsch: Zeigt, welcher Benutzer gerade aktiv ist.

### Result

```text
tim
```

---

## Test sudo access

### Command

```bash
sudo whoami
```

### Command breakdown

| Part     | Meaning                     | German              |
| -------- | --------------------------- | ------------------- |
| `sudo`   | superuser do / run as admin | als Admin ausführen |
| `whoami` | show current user           | wer bin ich         |

### What it does

Runs `whoami` with admin rights.

Deutsch: Führt `whoami` mit Adminrechten aus.

### Result

```text
root
```

### What the result means

The user `tim` can use admin rights with `sudo`.

Deutsch: `tim` darf kurz Adminrechte benutzen.

---

## Create SSH folder

### Command

```bash
mkdir -p ~/.ssh
```

### Command breakdown

| Part    | Meaning           | German                |
| ------- | ----------------- | --------------------- |
| `mkdir` | make directory    | Ordner erstellen      |
| `-p`    | create if missing | falls nötig erstellen |
| `~`     | home directory    | Benutzerordner        |
| `.ssh`  | SSH folder        | SSH-Ordner            |

### What it does

Creates the SSH folder inside the home directory of `tim`.

Deutsch: Erstellt den SSH-Ordner im Benutzerordner von `tim`.

### Why

Each Linux user has their own SSH key setup.

Deutsch: Jeder Benutzer hat seinen eigenen SSH-Bereich.

---

## Secure SSH folder permissions

### Command

```bash
chmod 700 ~/.ssh
```

### Command breakdown

| Part     | Meaning     | German        |
| -------- | ----------- | ------------- |
| `chmod`  | change mode | Rechte ändern |
| `700`    | owner only  | nur Besitzer  |
| `~/.ssh` | SSH folder  | SSH-Ordner    |

### What it does

Only the owner can access the `.ssh` folder.

Deutsch: Nur `tim` darf auf den SSH-Ordner zugreifen.

### Why

SSH may reject keys if the folder permissions are too open.

Deutsch: SSH kann Schlüssel ablehnen, wenn die Rechte zu offen sind.

---

## Check SSH folder permissions

### Command

```bash
ls -ld ~/.ssh
```

### Command breakdown

| Part     | Meaning          | German        |
| -------- | ---------------- | ------------- |
| `ls`     | list             | anzeigen      |
| `-l`     | long format      | Details       |
| `-d`     | directory itself | Ordner selbst |
| `~/.ssh` | SSH folder       | SSH-Ordner    |

### What it does

Shows the permissions of the `.ssh` folder itself.

Deutsch: Zeigt die Rechte vom SSH-Ordner selbst.

### Result

```text
drwx------ ... /home/tim/.ssh
```

---

## Add public SSH key

### Command

```bash
nano ~/.ssh/authorized_keys
```

### Command breakdown

| Part              | Meaning             | German                         |
| ----------------- | ------------------- | ------------------------------ |
| `nano`            | text editor         | Texteditor                     |
| `~/.ssh`          | SSH folder          | SSH-Ordner                     |
| `authorized_keys` | allowed public keys | erlaubte öffentliche Schlüssel |

### What it does

Opens the file where allowed public SSH keys are stored.

Deutsch: Öffnet die Datei für erlaubte öffentliche SSH-Schlüssel.

### Important note

The public key goes into `authorized_keys`.

Deutsch: Der öffentliche Schlüssel kommt auf den Server.

The private key stays on my local computer.

Deutsch: Der private Schlüssel bleibt auf meinem PC.

---

## Check key file line count

### Command

```bash
wc -l ~/.ssh/authorized_keys
```

### Command breakdown

| Part              | Meaning                  | German                       |
| ----------------- | ------------------------ | ---------------------------- |
| `wc`              | word count               | zählen                       |
| `-l`              | lines                    | Zeilen                       |
| `authorized_keys` | allowed public keys file | Datei mit erlaubten SSH-Keys |

### What it does

Checks how many lines are inside `authorized_keys`.

Deutsch: Prüft, wie viele Zeilen in der Datei sind.

### Result

```text
1
```

### What the result means

The public SSH key is stored as one line.

Deutsch: Der öffentliche SSH-Key steht als eine Zeile in der Datei.

---

## Secure authorized_keys permissions

### Command

```bash
chmod 600 ~/.ssh/authorized_keys
```

### Command breakdown

| Part              | Meaning                  | German                       |
| ----------------- | ------------------------ | ---------------------------- |
| `chmod`           | change mode              | Rechte ändern                |
| `600`             | owner read/write only    | nur Besitzer lesen/schreiben |
| `authorized_keys` | allowed public keys file | erlaubte SSH-Keys            |

### What it does

Only the owner can read and edit `authorized_keys`.

Deutsch: Nur `tim` darf die Datei lesen und bearbeiten.

---

## Check authorized_keys permissions

### Command

```bash
ls -l ~/.ssh/authorized_keys
```

### Command breakdown

| Part              | Meaning                  | German                       |
| ----------------- | ------------------------ | ---------------------------- |
| `ls`              | list                     | anzeigen                     |
| `-l`              | long format              | Details                      |
| `authorized_keys` | allowed public keys file | Datei mit erlaubten SSH-Keys |

### Result

```text
-rw------- ... /home/tim/.ssh/authorized_keys
```

### What the result means

The file is private and only accessible by `tim`.

Deutsch: Die Datei ist geschützt und nur für `tim` zugänglich.

---

## Set server timezone

### Command

```bash
sudo timedatectl set-timezone Europe/Berlin
```

### Command breakdown

| Part            | Meaning                     | German               |
| --------------- | --------------------------- | -------------------- |
| `sudo`          | superuser do / run as admin | als Admin ausführen  |
| `timedatectl`   | manage system time          | Systemzeit verwalten |
| `set-timezone`  | set timezone                | Zeitzone setzen      |
| `Europe/Berlin` | German timezone             | deutsche Zeitzone    |

### What it does

Sets the server timezone to Europe/Berlin.

Deutsch: Stellt die Server-Zeitzone auf Deutschland/Berlin.

---

## Check server time

### Command

```bash
date
```

### Command breakdown

| Part   | Meaning            | German                     |
| ------ | ------------------ | -------------------------- |
| `date` | show date and time | Datum und Uhrzeit anzeigen |

### Result

```text
CEST
```

### What the result means

The server now uses German summer time.

Deutsch: Der Server nutzt jetzt deutsche Sommerzeit.
