## Initial user and SSH setup

### Create a new user

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
groups tim
```

### Command breakdown

| Part      | Meaning     | German          |
| --------- | ----------- | --------------- |
| `user`    | user        | Benutzer        |
| `mod`     | modify      | ändern          |
| `usermod` | modify user | Benutzer ändern |
| `-a`      | append      | hinzufügen      |
| `-G`      | groups      | Gruppen         |
| `sudo`    | admin group | Admin-Gruppe    |
| `tim`     | username    | Benutzername    |

### What it does

Adds `tim` to the `sudo` group.

Deutsch: `tim` bekommt Adminrechte über die Gruppe `sudo`.

### Result

```text
tim : tim sudo users
```

---

## Switch to the new user

### Command

```bash
su - tim
whoami
```

### Command breakdown

| Part     | Meaning           | German                |
| -------- | ----------------- | --------------------- |
| `su`     | substitute user   | Benutzer wechseln     |
| `-`      | login shell       | vollständige Umgebung |
| `tim`    | target user       | Zielbenutzer          |
| `whoami` | show current user | wer bin ich           |

### What it does

Switches from `root` to the user `tim`.

Deutsch: Wechselt zum Benutzer `tim`.

### Result

```text
tim
```

---

## Test sudo rights

### Command

```bash
sudo whoami
```

### Command breakdown

| Part     | Meaning           | German              |
| -------- | ----------------- | ------------------- |
| `sudo`   | run as admin      | als Admin ausführen |
| `whoami` | show current user | wer bin ich         |

### What it does

Checks if `tim` can run commands with admin rights.

Deutsch: Prüft, ob `tim` Adminrechte benutzen darf.

### Result

```text
root
```

---

## Prepare SSH key folder

### Command

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
ls -ld ~/.ssh
```

### Command breakdown

| Part    | Meaning           | German                |
| ------- | ----------------- | --------------------- |
| `mkdir` | make directory    | Ordner erstellen      |
| `-p`    | create if missing | falls nötig erstellen |
| `~`     | home directory    | Benutzerordner        |
| `.ssh`  | SSH folder        | SSH-Ordner            |
| `chmod` | change mode       | Rechte ändern         |
| `700`   | owner only        | nur Besitzer          |
| `ls`    | list              | anzeigen              |
| `-l`    | long format       | Details               |
| `-d`    | directory itself  | Ordner selbst         |

### What it does

Creates and secures the SSH folder for the user `tim`.

Deutsch: Erstellt und schützt den SSH-Ordner von `tim`.

### Result

```text
drwx------ ... /home/tim/.ssh
```

---

## Add public SSH key

### Command

```bash
nano ~/.ssh/authorized_keys
wc -l ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
ls -l ~/.ssh/authorized_keys
```

### Command breakdown

| Part              | Meaning               | German                         |
| ----------------- | --------------------- | ------------------------------ |
| `nano`            | text editor           | Texteditor                     |
| `authorized_keys` | allowed public keys   | erlaubte öffentliche Schlüssel |
| `wc`              | word count            | zählen                         |
| `-l`              | lines                 | Zeilen                         |
| `chmod 600`       | owner read/write only | nur Besitzer lesen/schreiben   |

### What it does

Adds the public SSH key to the list of keys allowed to log in as `tim`.

Deutsch: Der öffentliche SSH-Key wird für den Login als `tim` erlaubt.

### Result

```text
1 /home/tim/.ssh/authorized_keys
-rw------- ... /home/tim/.ssh/authorized_keys
```

Note: The real public key is not documented here.

---

## Set server timezone

### Command

```bash
sudo timedatectl set-timezone Europe/Berlin
date
```

### Command breakdown

| Part            | Meaning            | German               |
| --------------- | ------------------ | -------------------- |
| `sudo`          | run as admin       | als Admin            |
| `timedatectl`   | manage system time | Systemzeit verwalten |
| `set-timezone`  | set timezone       | Zeitzone setzen      |
| `Europe/Berlin` | German timezone    | deutsche Zeitzone    |
| `date`          | show date/time     | Datum/Zeit anzeigen  |

### What it does

Changes the server timezone from UTC to Europe/Berlin.

Deutsch: Stellt die Serverzeit auf deutsche Zeit.

### Result

```text
CEST
```
