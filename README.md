# Tutorial Projekt mit CI/CD

In diesem Tutorial geht es darum, ein einfaches Projekt inklusive CI/CD in GitHub aufzusetzetzen.
Eingesetzt dafür werden:
- Git
- Node.js
- React mit Typescript
- GitHub Actions
- Docker
- GitHub Pages

## Voraussetzungen
Die diversen Softwarepakete welche eingesetzt werden, werden im Verlauf des Tutorials installiert. Wer das im voraus machen möchte, folgende Software wird eingesetzt:
* GIT (https://git-scm.com/downloads)
* Node.js (https://nodejs.org)
* Docker (https://www.docker.com/get-started)

Alle eingesetzte Software ist Plattformunabhängig und funktioniert auf Windows/Linux/OSX. Ich persönlich empfehle, Linux zu verwenden. Insbesondere Docker läuft unter Linux wesentlich besser als auf Windows oder OSX.

## GIT Installieren
### Windows:
Auf https://git-scm.com/downloads den Installer herunterladen und alles installieren (die Default Einstellungen sind in Ordnung).

Nachher am besten immer die GIT Bash vom Startmenu für die weiteren Befehle verwenden. Die GIT Bash ist Linux-Kompatibel. Mir sind Befehle in Zusammenhang mit Node.js bekannt, welche in der Vergangenheit Probleme gemacht wenn die PowerShell oder CMD verwendet wird.
Ausserdem sind die Befehle, welche später im Zusammenhang mit Docker verwendet werden ebenfalls Bash-Shell kompatibel. Von daher macht es sowieso sinn, sich damit vertraut zu machen.

### Linux (Debian/Ubuntu)
```console
$ sudo apt install git
```


## Node.js Installieren
### Windows:
Auf https://nodejs.org den Installer herunterladen und alles installieren (die Default Einstellungen sind in Ordnung).

Nachher ausloggen oder den Computer neustarten. Erst nachher wird die Umgebungsvariable PATH neu geladen, welche im Installationsvorgang (bei GIT und Node.js) angepasst wird.

### Linux (Debian/Ubuntu)
Der Anleitung auf https://nodejs.org/en/download/package-manager/ befolgen.
Für Debian/Ubuntu
```console
$ curl -fsSL https://deb.nodesource.com/setup_14.x | sudo -E bash -
$ sudo apt-get install -y nodejs
```

## React/Typescript Projekt initialisieren
Wer das ganze im Detail mit mehr Zusatzinfos machen möchte. Dieses Tutorial macht im Grunde das, was unter https://create-react-app.dev/docs/adding-typescript/ beschrieben ist.

```console
$ cd /path/to/project/folder
$ npx create-react-app vier-gewinnt --template typescript
$ cd vier-gewinnt
$ git log
commit 52f4b0f31d459b88186817f35dfa0940b2864e5a (HEAD -> master)
Author: Adrian Imboden <adi@thingdust.com>
Date:   Wed Feb 17 13:13:44 2021 +0000

    Initialize project using Create React App
```

Jetzt haben wir ein leeres Typescript React.js Projekt. Der `git log` Befehl zeigt, dass unser GIT Repository einen Commit drin hat.


## GIT Konfigurieren
Jetzt auf https://github.com registerieren. Üblicherweise verwendet man GIT über SSH, deshalb machen wir noch einen SSH Key für die Authentifizierung:
```console
$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/user/.ssh/id_rsa): 
Created directory '/home/user/.ssh'.
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/user/.ssh/id_rsa
Your public key has been saved in /home/user/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:d3BQ9TcRtqlnhgBpfHQc9HAvUabOWBnt3tGv4zgKHiE root@3d1630f40a0c
The key's randomart image is:
+---[RSA 3072]----+
|        ..+o+*=B+|
|         +.o..=B*|
|        . o.. =*=|
|           o.=oo=|
|      E S . ooo=+|
|       . o .  + +|
|        o      . |
|       . o  ..o  |
|        . ...o.. |
+----[SHA256]-----+
$ cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDYzuwNL6MEp3UQzg9XtuZQsdtfG2mHCnstOFMQmdzAicKu03QhDWak5cyq/2UZxCgLYpERto60GvaQQeOOjv0cvlOGksvA/4wVpxH9yaHxZv+o5+XCCP/dt2zZ5vMxDjGwQFJy9NvQm7tGW7B6PPl38fmNH006nGpJNpXdkMs4Zuc9HuursHPIotoLolXTlgTB8W0RAm2Jr4uyuTJ6GJo6e2gWLBn4HMX9Kg0zQFL4LshUnjb5nCRUCu6M9Z5wvAniumSLnDtIp3pUm8gO8O/MbDUQKHwOsBumII8LgHyh60E9NnEibv2pVAV5APactzVbSnAMQIb1RiMcjV4YaLUKjhAz1djOYrj7QgLIxqeZAukABMP7b2iJQu8glVBMQQ3XvQK9EkQmY4wQqRFJcR5hZj/HzXPqZmsbaikmEZP/4vp2mwxkJgZ3rgiS/mva3kINg39L8z2J5FfCDSsk7CieVjiMD+lG2ceWvMRCydL24XsMkefHsHaO9mPjo5BTlgs= user@computer
```
Den mit `cat ~/.ssh/id_rsa.pub` ausgegebenen Key kann man jetzt auf GitHub (https://github.com/settings/keys) registrieren.

Zum ausprobieren ob alles funktioniert hat, kannst du ja dieses Tutorial repo mal Klonen:
```console
$ cd /tmp
$ git clone git@github.com:adrianimboden/tutorial-ci-cd.git
Cloning into 'tutorial-ci-cd'...
The authenticity of host 'github.com (140.82.121.3)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com,140.82.121.3' (RSA) to the list of known hosts.
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

Das erste mal muss man den github.com Endpunkt bestätigen. Falls später mal eine Man-in-the-middle stattfinden würde, dann würde GIT/SSH das bemerken.


## Git repo hochladen
Jetzt auf https://github.com ein neues Repository erstellen.
Für dieses Tutorial habe ich https://github.com/adrianimboden/vier_gewinnt gewählt.

Jetzt im Projektordner von vorher:
```console
$ cd /path/to/project/folder/vier-gewinnt
$ git push --set-upstream git@github.com:adrianimboden/vier_gewinnt.git master
Enumerating objects: 24, done.
Counting objects: 100% (24/24), done.
Delta compression using up to 64 threads
Compressing objects: 100% (23/23), done.
Writing objects: 100% (24/24), 186.85 KiB | 2.15 MiB/s, done.
Total 24 (delta 0), reused 0 (delta 0)
To github.com:adrianimboden/vier-gewinnt.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'git@github.com:adrianimboden/vier-gewinnt.git'.
```

Jetzt ist auf https://github.com/adrianimboden/vier_gewinnt der erste Commit, welcher von `npx create-react-app` erstellt wurde, zu sehen.


## Build und tests lokal ausführen
Damit wir lokal und auf dem Build-Server die gleiche Umgebung haben, bietet sicher hier Docker an. Aber zuerst machen wir mal von Hand, was wir nachher automatisieren wollen:

```console
$ cd /path/to/project/folder/vier-gewinnt
> vier-gewinnt@0.1.0 build /path/to/project/folder/vier-gewinnt
> react-scripts build

Creating an optimized production build...
Compiled successfully.

File sizes after gzip:

  41.34 KB  build/static/js/2.50a60a52.chunk.js
  1.59 KB   build/static/js/3.ab7c31bf.chunk.js
  1.17 KB   build/static/js/runtime-main.4d4cba2f.js
  596 B     build/static/js/main.d612ff39.chunk.js
  531 B     build/static/css/main.8c8b27cf.chunk.css

The project was built assuming it is hosted at /.
You can control this with the homepage field in your package.json.

The build folder is ready to be deployed.
You may serve it with a static server:

  npm install -g serve
  serve -s build

Find out more about deployment here:

  https://cra.link/deployment
$ CI=true npm test

> vier-gewinnt@0.1.0 test /home/thingdust/src/projects/tutorial/vier-gewinnt
> react-scripts test

PASS src/App.test.tsx
  ✓ renders learn react link (20 ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        1.031 s
Ran all test suites.
```

Jetzt ist im Ordner build die statische Webseite, welche wir irgendwo hochladen könnten und die Tests sind erfolgreich durchgelaufen.

## Build und tests mit Docker ausführen
Jetzt wollen wir das ganze noch mit Docker ausführen. Docker kann man als leichtgewichtige virtuelle Maschine ansehen. Mittels eines Dockerfiles automatisieren wir den Ablauf aus dem vorherigen Schritt nun.

Wir legen eine Datei mit dem Namen "Dockerfile" (keine Dateiendung) an, mit folgendem Inhalt:
```Dockerfile
FROM ubuntu:focal

RUN apt-get update && apt-get install -y curl
RUN curl -fsSL https://deb.nodesource.com/setup_14.x | bash -
RUN apt-get install -y nodejs

WORKDIR /src

COPY package.json /src/package.json
COPY package-lock.json /src/package-lock.json

RUN npm install

COPY . /src/
RUN npm run-script build
RUN CI=true npm test
```

Das Dockerfile können wir jetzt ausführen:
```console
$ cd /path/to/project/folder/vier-gewinnt
$ docker build .
```

Das Dockerfile im Detail:
Befehl | Beschreibung
------------------------------------------------- | -------------
| `FROM ubuntu:focal`                             | Wir nehmen als Basis ein fertig installiertes Ubuntu in der Version focal |
| `RUN apt-get update && apt-get install -y curl` | curl installieren (wird für die Installation von Node.js verwendet |
| ```RUN curl -fsSL https://deb.nodesource.com/setup_14.x \| bash -```<br>`RUN apt-get install -y nodejs` | Node.js intallieren (das ist der Befehl vom Tutorial am Anfang) |
| `WORKDIR /src` | Ab jetzt arbeiten wir im Pfad /src (ist das selbe wie `cd /src`, aber das würde im Dockerfile nur für ein einzelnes RUN gelten) |
| `COPY package.json /src/package.json`<br>`COPY package-lock.json /src/package-lock.json` | Für das installieren der Projekt-Dependencies brauchen wir nur package.json und package-lock.json. Wir könnten auch direkt auf dem ganzen Source arbeiten, das würde dann aber bedeuten dass wir bei jeder kleinsten Code-Änderung immer wieder alle Abhängigkeiten von Null aufbauen müssten. |
| `RUN npm install` | Damit installieren wir die Projektabhängigkeiten. Das wurde im Tutorial automatisch von `npx create-react-app` gemacht. |
| `COPY . /src/` | Jetzt kopieren wir unseren gesamten Source in Docker hinein. |
| `RUN npm run-script build`<br>`RUN CI=true npm test` | Und zum Schluss der eigentlich Build- und Testablauf, wie wir ihn vorher von Hand gemacht haben |

## node_modules
Wer vorher genau hingeschaut hat, hat vielleicht bemerkt dass beim Befehl `docker build .` ca. 250MiB an Daten an den Docker daemon gesendet werden. Das ist ja viel mehr, als unser Code gross ist. Der Grund ist, dass alle Projektabhängigkeiten (der node_modules Ordner) mit hineinkopiert wird. Dieser ist aber nicht Teil des GIT Repositories und wird auf dem Buildserver auch nicht vorhanden sein.

Deshalb erstellen wir noch eine Datei mit dem Namen `.dockerignore` (analog zu .gigignore) mit folgendem Inhalt:
```
node_modules/**
```

Der Befehl `RUN npm install` erstellt diesen Ordner dann


## Die Änderungen auf GitHub bringen

```console
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .dockerignore
        Dockerfile

nothing added to commit but untracked files present (use "git add" to track)
$ git add .
$ git diff --staged
... anzeige was geändert hat ...
$ git commit -m"added Dockerfile for ci build"
[master c19e869] added Dockerfile for ci build
 2 files changed, 17 insertions(+)
 create mode 100644 .dockerignore
 create mode 100644 Dockerfile
$ git push
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 64 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (4/4), 547 bytes | 547.00 KiB/s, done.
Total 4 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To github.com:adrianimboden/tmp-vier-gewinnt.git
   52f4b0f..c19e869  master -> master
```

Hier sieht man ein paar nützliche git Kommandos:
| Befehl | Beschreibung |
| ------ | ------------ |
| `git status` | Anzeigen, was für Dateien wir seit letztem mal editiert/hinzugefügt/gelöscht haben |
| `git add` | Geänderte, Gelöschte und Hinzugefügte Dateien "stagen" |
| `git diff --staged` | Die Differenz zwischen dem neusten Commit (HEAD) und dem "stage"-Bereich anzeigen |
| `git commit -m"Beschreibung"` | Den "stage"-Bereich mit einer Beschreibung abspeichern |
| `git push` | Die lokalen Commits welche noch nicht auf dem Server sind hochladen |

Ein gutes Tutorial, für jene welche GIT verstehen möchten: https://www.sbf5.com/~cduan/technical/git/



## GitHub Action einrichten
GitHub Actions machen, dass wir bei jeder Änderung das Dockerfile builden können. Das ist der erste Schritt für ein sauberes CI.

Wir erstellen folgende Datei und Ordnerstruktur im Repository:
.github/workflows/ci.yaml

mit folgendem Inhalt:
```
name: CI only

on:
  pull_request:
  push:


jobs:
  build:
    runs-on: ubuntu-20.04

    steps:
      - uses: actions/checkout@v2
      - run: docker build .
```

Diese minimale Konfiguration macht dass:
- Bei jedem Pull-Request (dazu später mehr)
- und bei jedem Push
die Jobs ausgeführt werden.

Der Job selber läuft auf einer Ubuntu 20.04 Maschine, welche von GitHub gestellt wird. Wenn das erstelle Repo ein Public-Repository ist, dann gibt es keine Einschränkungen bezüglich der genutzten CPU-Zeit etc.

Das ganze jetzt wieder wie gehabt auf github hochladen (`git add .`, `git commit -m"..."`, `git push`), danach können wir der Action auf github zuschauen
