# Docker installation

This file documents the Docker installation on my Ubuntu server.

---

## Why this setup is done before installing Docker

Before Docker can be installed from the official Docker repository, `apt` must know and trust the Docker package source.

Deutsch: Bevor Docker installiert wird, muss `apt` wissen, welcher Docker-Quelle es vertrauen darf.

The setup flow is:

1. install required tools
2. create the apt keyrings folder
3. download the Docker signing key
4. add the Docker apt source
5. refresh package lists
6. install Docker

Deutsch: Erst wird die Docker-Quelle vorbereitet, dann wird Docker installiert.

---

## Install required tools

### Command

```bash
sudo apt install ca-certificates curl gnupg -y
```

### Command breakdown

| Part              | Meaning                            | German                                 |
| ----------------- | ---------------------------------- | -------------------------------------- |
| `sudo`            | superuser do / run as admin        | als Admin ausführen                    |
| `apt`             | Advanced Package Tool              | Paketverwaltung                        |
| `install`         | install software                   | installieren                           |
| `ca-certificates` | Certificate Authority certificates | Zertifikate vertrauenswürdiger Stellen |
| `curl`            | client URL / URL transfer tool     | Daten von URLs laden                   |
| `gnupg`           | GNU Privacy Guard                  | Schlüssel/Signaturen prüfen            |
| `-y`              | yes automatically                  | automatisch ja                         |

### What it does

Installs tools needed before Docker can be installed securely.

Deutsch: Installiert Werkzeuge für die sichere Docker-Installation.

### Notes

`ca-certificates` helps the system trust HTTPS connections.

Deutsch: Prüft HTTPS-Vertrauen.

`curl` downloads data from URLs.

Deutsch: Lädt Daten von Links/URLs.

`gnupg` handles cryptographic keys and signatures.

Deutsch: Prüft/verarbeitet Schlüssel und Signaturen.

### Result

```text
ca-certificates is already the newest version.
curl is already the newest version.
gnupg is already the newest version.
0 upgraded, 0 newly installed, 0 to remove and 1 not upgraded.
```

---

## Create apt keyrings folder

### Command

```bash
sudo install -m 0755 -d /etc/apt/keyrings
```

### Command breakdown

| Part                | Meaning                      | German                         |
| ------------------- | ---------------------------- | ------------------------------ |
| `sudo`              | superuser do / run as admin  | als Admin ausführen            |
| `install`           | create/copy with permissions | erstellen/kopieren mit Rechten |
| `-m`                | mode                         | Rechte-Modus                   |
| `0755`              | permission value             | Rechtewert                     |
| `-d`                | directory                    | Ordner                         |
| `/etc/apt/keyrings` | apt key storage folder       | Schlüsselordner für apt        |

### What it does

Creates the folder `/etc/apt/keyrings` with `0755` permissions.

Deutsch: Erstellt den Ordner `/etc/apt/keyrings` mit passenden Rechten.

### Permission breakdown

| Symbol | Meaning                                         | German                                |
| ------ | ----------------------------------------------- | ------------------------------------- |
| `r`    | read                                            | lesen                                 |
| `w`    | write                                           | schreiben                             |
| `x`    | execute for files / enter directory for folders | ausführen / Ordner betreten           |
| `0755` | `rwxr-xr-x`                                     | Besitzer alles, andere lesen/betreten |

### Result

```text
drwxr-xr-x 2 root root 4096 Mar 31 2024 /etc/apt/keyrings
```

### What it means

The keyrings folder exists.
It is owned by `root`, and other users can read/enter it but not modify it.

Deutsch: Der Ordner existiert. Nur `root` darf ihn verändern.

---

## Download Docker signing key

### Command

```bash
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
```

### Command breakdown

| Part                           | Meaning                     | German                   |
| ------------------------------ | --------------------------- | ------------------------ |
| `sudo`                         | superuser do / run as admin | als Admin ausführen      |
| `curl`                         | download from URL           | von URL laden            |
| `-f`                           | fail on server errors       | bei Fehler abbrechen     |
| `-s`                           | silent mode                 | weniger Ausgabe          |
| `-S`                           | show errors                 | Fehler trotzdem anzeigen |
| `-L`                           | follow redirects            | Weiterleitungen folgen   |
| URL                            | Docker GPG key source       | Docker-Schlüsselquelle   |
| `-o`                           | output file                 | speichern als            |
| `/etc/apt/keyrings/docker.asc` | Docker signing key file     | Docker-Schlüsseldatei    |

### What it does

Downloads the official Docker signing key and saves it as `docker.asc`.

Deutsch: Lädt den Docker-Signaturschlüssel herunter und speichert ihn als `docker.asc`.

### Result

No output.

Deutsch: Keine Ausgabe ist hier normal, weil `-s` für silent mode genutzt wird.

---

## Check Docker signing key file

### Command

```bash
ls -l /etc/apt/keyrings/docker.asc
```

### Result

```text
-rw-r--r-- 1 root root 3817 Jun 2 23:21 /etc/apt/keyrings/docker.asc
```

### What it means

The Docker signing key exists and is readable.
Only `root` can modify it.

Deutsch: Der Docker-Schlüssel existiert. Nur `root` darf ihn verändern.

---

## Make Docker key readable

### Command

```bash
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

### Command breakdown

| Part         | Meaning                     | German                |
| ------------ | --------------------------- | --------------------- |
| `sudo`       | superuser do / run as admin | als Admin ausführen   |
| `chmod`      | change mode                 | Rechte ändern         |
| `a`          | all                         | alle                  |
| `+`          | add permission              | Recht hinzufügen      |
| `r`          | read                        | lesen                 |
| `docker.asc` | Docker key file             | Docker-Schlüsseldatei |

### What it does

Makes the Docker signing key readable for `apt`.

Deutsch: Macht den Docker-Schlüssel für `apt` lesbar.

### Note

The Docker signing key is public.
It is okay that users can read it.
Only `root` should be able to modify it.

Deutsch: Der Schlüssel ist öffentlich lesbar, aber nur `root` darf ihn verändern.

---

## Check server architecture

### Command

```bash
dpkg --print-architecture
```

### Command breakdown

| Part                   | Meaning               | German               |
| ---------------------- | --------------------- | -------------------- |
| `dpkg`                 | Debian package tool   | Debian-Paketwerkzeug |
| `--print-architecture` | show CPU architecture | Architektur anzeigen |

### Result

```text
amd64
```

### What it means

The server uses the `amd64` architecture.

Deutsch: Der Server nutzt die `amd64`-Architektur.

---

## Check Ubuntu codename

### Command

```bash
. /etc/os-release && echo "$VERSION_CODENAME"
```

### Command breakdown

| Part                | Meaning                        | German                             |
| ------------------- | ------------------------------ | ---------------------------------- |
| `.`                 | source file                    | Datei einlesen                     |
| `/etc/os-release`   | operating system info          | Betriebssystem-Infos               |
| `&&`                | run next command if successful | danach ausführen, wenn erfolgreich |
| `echo`              | print text                     | Text ausgeben                      |
| `$VERSION_CODENAME` | Ubuntu codename                | Ubuntu-Codename                    |

### Result

```text
noble
```

### What it means

The server runs Ubuntu with the codename `noble`.

Deutsch: Der Server nutzt Ubuntu-Codename `noble`.

---

## Add Docker apt source

### Command

```bash
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF
```

### Command breakdown

| Part                                     | Meaning                     | German                    |
| ---------------------------------------- | --------------------------- | ------------------------- |
| `sudo`                                   | superuser do / run as admin | als Admin ausführen       |
| `tee`                                    | write text into a file      | Text in Datei schreiben   |
| `/etc/apt/sources.list.d/docker.sources` | Docker apt source file      | Docker-Paketquellen-Datei |
| `<<EOF ... EOF`                          | text block                  | Textblock                 |
| `Types: deb`                             | Debian package type         | Debian-Pakete             |
| `URIs`                                   | repository URL              | Paketquellen-Link         |
| `Suites`                                 | Ubuntu codename             | Ubuntu-Version            |
| `Components: stable`                     | stable Docker packages      | stabile Pakete            |
| `Architectures`                          | CPU architecture            | CPU-Architektur           |
| `Signed-By`                              | signing key path            | Signaturschlüssel         |

### What it does

Adds the official Docker package source for `apt`.

Deutsch: Fügt die offizielle Docker-Paketquelle für `apt` hinzu.

### Key idea

`docker.asc` says who `apt` should trust.
`docker.sources` says where `apt` can find Docker.

Deutsch: `docker.asc` ist der Vertrauensschlüssel. `docker.sources` ist die Docker-Adresse für `apt`.

---

## Check Docker apt source

### Command

```bash
cat /etc/apt/sources.list.d/docker.sources
```

### Command breakdown

| Part                                     | Meaning                | German                    |
| ---------------------------------------- | ---------------------- | ------------------------- |
| `cat`                                    | show file content      | Dateiinhalt anzeigen      |
| `/etc/apt/sources.list.d/docker.sources` | Docker apt source file | Docker-Paketquellen-Datei |

### Result

```text
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: noble
Components: stable
Architectures: amd64
Signed-By: /etc/apt/keyrings/docker.asc
```

### What it means

`apt` now knows where to find Docker packages and which signing key to use.

Deutsch: `apt` weiß jetzt, wo Docker liegt und welcher Schlüssel genutzt wird.

---

## Refresh package lists

### Command

```bash
sudo apt update
```

### Command breakdown

| Part     | Meaning                     | German                    |
| -------- | --------------------------- | ------------------------- |
| `sudo`   | superuser do / run as admin | als Admin ausführen       |
| `apt`    | Advanced Package Tool       | Paketverwaltung           |
| `update` | refresh package lists       | Paketlisten aktualisieren |

### What it does

Refreshes the local package index.
It does not install updates.

Deutsch: Aktualisiert die Paketliste, installiert aber noch nichts.

### Why again?

A new Docker package source was added.
`apt update` reads the new source and checks available packages.

Deutsch: Neue Paketquelle hinzugefügt, also muss `apt` die Listen neu einlesen.

### Result

```text
Fetched 16.1 MB in 2s
Reading package lists... Done
5 packages can be upgraded.
```

---

## Check upgradable packages

### Command

```bash
apt list --upgradable
```

### Command breakdown

| Part           | Meaning                         | German                 |
| -------------- | ------------------------------- | ---------------------- |
| `apt`          | Advanced Package Tool           | Paketverwaltung        |
| `list`         | list packages                   | Pakete anzeigen        |
| `--upgradable` | packages with available updates | aktualisierbare Pakete |

### What it does

Shows installed packages that can be upgraded.

Deutsch: Zeigt installierte Pakete, für die Updates verfügbar sind.

### Result

```text
apparmor ... [upgradable from: ...]
cloud-init ... [upgradable from: ...]
libapparmor1 ... [upgradable from: ...]
liblzma5 ... [upgradable from: ...]
xz-utils ... [upgradable from: ...]
```

---

## Upgrade existing packages

### Command

```bash
sudo apt upgrade -y
```

### Command breakdown

| Part      | Meaning                           | German               |
| --------- | --------------------------------- | -------------------- |
| `sudo`    | superuser do / run as admin       | als Admin ausführen  |
| `apt`     | Advanced Package Tool             | Paketverwaltung      |
| `upgrade` | install available package updates | Updates installieren |
| `-y`      | yes automatically                 | automatisch ja       |

### What it does

Installs available updates for already installed packages.

Deutsch: Installiert Updates für bereits installierte Pakete.

### Result

```text
Running kernel seems to be up-to-date.
Restarting services...
No containers need to be restarted.
User sessions running outdated binaries: tim ...
```

---

## Reboot after upgrades

### Command

```bash
sudo reboot
```

### Command breakdown

| Part     | Meaning                     | German              |
| -------- | --------------------------- | ------------------- |
| `sudo`   | superuser do / run as admin | als Admin ausführen |
| `reboot` | restart the system          | Server neu starten  |

### What it does

Restarts the server.

Deutsch: Startet den Server neu.

### Why

After updates, some sessions or services may still use old loaded binaries.
A reboot makes sure the updated system is fully active.

Deutsch: Nach Updates können alte Programmteile noch geladen sein. Ein Neustart lädt alles sauber neu.

### Result

```text
Server restarted successfully.
```

---

## Install Docker

### Command

```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```

### Command breakdown

| Part                    | Meaning                           | German                    |
| ----------------------- | --------------------------------- | ------------------------- |
| `sudo`                  | superuser do / run as admin       | als Admin ausführen       |
| `apt`                   | Advanced Package Tool             | Paketverwaltung           |
| `install`               | install software                  | installieren              |
| `docker-ce`             | Docker Community Edition / Engine | Docker Engine             |
| `docker-ce-cli`         | Docker command line               | Docker-Befehle            |
| `containerd.io`         | container runtime                 | Container-Laufzeit        |
| `docker-buildx-plugin`  | build Docker images               | Docker-Images bauen       |
| `docker-compose-plugin` | manage multi-container apps       | mehrere Container steuern |
| `-y`                    | yes automatically                 | automatisch ja            |

### What it does

Installs Docker Engine, Docker CLI, container runtime, Buildx and Docker Compose.

Deutsch: Installiert Docker Engine, Docker-Befehle, Container-Laufzeit, Buildx und Docker Compose.

### Result

```text
Running kernel seems to be up-to-date.
No services need to be restarted.
No containers need to be restarted.
No user sessions are running outdated binaries.
```

---

## Check Docker service

### Command

```bash
sudo systemctl status docker --no-pager
```

### Command breakdown

| Part         | Meaning                     | German                |
| ------------ | --------------------------- | --------------------- |
| `sudo`       | superuser do / run as admin | als Admin ausführen   |
| `systemctl`  | control system services     | Systemdienste steuern |
| `status`     | show status                 | Status anzeigen       |
| `docker`     | Docker service              | Docker-Dienst         |
| `--no-pager` | show directly               | direkt anzeigen       |

### Result

```text
Active: active (running) since Wed 2026-06-03 00:04:23 CEST
```

### What it means

Docker is running as a system service.

Deutsch: Docker läuft als Systemdienst.

---

## Check disk usage

### Command

```bash
df -h
```

### Command breakdown

| Part | Meaning        | German                 |
| ---- | -------------- | ---------------------- |
| `df` | disk free      | Speicherplatz anzeigen |
| `-h` | human readable | lesbar in GB/MB        |

### Result

```text
/dev/vda1       8.7G  2.6G  6.2G  30% /
```

### What it means

The main disk uses 2.6 GB of 8.7 GB.

Deutsch: Die Hauptfestplatte nutzt 2.6 GB von 8.7 GB.

---

## Check RAM

### Command

```bash
free -h
```

### Command breakdown

| Part   | Meaning        | German          |
| ------ | -------------- | --------------- |
| `free` | memory usage   | RAM anzeigen    |
| `-h`   | human readable | lesbar in GB/MB |

### Result

```text
Mem: 826Mi total, 413Mi used, 413Mi available
Swap: 0B
```

### What it means

The server has limited RAM, but Docker can be used for small test projects.

Deutsch: Der Server hat wenig RAM, aber Docker reicht für kleine Tests.

---

## Check Docker version

### Command

```bash
docker --version
```

### Command breakdown

| Part        | Meaning                  | German           |
| ----------- | ------------------------ | ---------------- |
| `docker`    | Docker command line tool | Docker-Befehl    |
| `--version` | show installed version   | Version anzeigen |

### Result

```text
Docker version 29.5.2, build 79eb04c
```

### What it means

Docker CLI is installed and available.

Deutsch: Docker ist installiert und im Terminal verfügbar.

---

## Run Docker test container

### Command

```bash
sudo docker run hello-world
```

### Command breakdown

| Part          | Meaning                     | German              |
| ------------- | --------------------------- | ------------------- |
| `sudo`        | superuser do / run as admin | als Admin ausführen |
| `docker`      | Docker command              | Docker-Befehl       |
| `run`         | start a container           | Container starten   |
| `hello-world` | test image                  | Test-Image          |

### What it does

Starts a test container from the `hello-world` image.

Deutsch: Startet einen Testcontainer aus dem `hello-world` Image.

### Result

```text
Unable to find image 'hello-world:latest' locally
Pulling from library/hello-world
Pull complete
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

### What it means

Docker successfully downloaded and started a test container.

Deutsch: Docker konnte einen Testcontainer herunterladen und starten.

---

## Show all containers

### Command

```bash
sudo docker ps -a
```

### Command breakdown

| Part     | Meaning                     | German              |
| -------- | --------------------------- | ------------------- |
| `sudo`   | superuser do / run as admin | als Admin ausführen |
| `docker` | Docker command              | Docker-Befehl       |
| `ps`     | process status              | Prozessstatus       |
| `-a`     | all                         | alle anzeigen       |

### Result

```text
IMAGE         STATUS
hello-world   Exited (0)
```

### What it means

The `hello-world` test container ran successfully and exited with code `0`.

Deutsch: Der Testcontainer lief erfolgreich und wurde sauber beendet.

---

## Add user to Docker group

### Command

```bash
sudo usermod -aG docker tim
```

### Command breakdown

| Part      | Meaning                     | German              |
| --------- | --------------------------- | ------------------- |
| `sudo`    | superuser do / run as admin | als Admin ausführen |
| `usermod` | modify user                 | Benutzer ändern     |
| `-a`      | append                      | hinzufügen          |
| `-G`      | groups                      | Gruppen             |
| `docker`  | Docker group                | Docker-Gruppe       |
| `tim`     | username                    | Benutzername        |

### What it does

Adds `tim` to the `docker` group.

Deutsch: Fügt `tim` zur Docker-Gruppe hinzu.

### Security note

The `docker` group is powerful.
It should only be given to trusted admin users.

Deutsch: Die Docker-Gruppe ist mächtig und sollte nur vertrauenswürdigen Admin-Usern gegeben werden.

---

## Check user groups

### Command

```bash
groups tim
```

### Result

```text
tim : tim sudo users docker
```

### What it means

The user `tim` has Docker group access.

Deutsch: `tim` darf Docker über die Docker-Gruppe benutzen.

---

## Test Docker without sudo

### Command

```bash
docker ps
```

### Command breakdown

| Part     | Meaning        | German        |
| -------- | -------------- | ------------- |
| `docker` | Docker command | Docker-Befehl |
| `ps`     | process status | Prozessstatus |

### Result

```text
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

### What it means

Docker works without `sudo`.
No containers are currently running.

Deutsch: Docker funktioniert ohne `sudo`. Aktuell läuft kein Container.

---

## Summary

Docker is installed and working.

* Docker service is active
* Docker CLI is installed
* `hello-world` test container worked
* `tim` is in the `docker` group
* Docker works without `sudo`

Deutsch: Docker ist installiert, getestet und für den Benutzer `tim` nutzbar.
