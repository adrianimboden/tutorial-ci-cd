# Tutorial Projekt mit CI/CD

Dieses Tutorial zeigt, wie man ein einfaches Projekt inklusive CI/CD in GitHub aufsetzen kann.

## Inhaltsverzeichnis

* [Technologien](#Technologien)

## Technologien

* **Git**\
  _ein open sorce Versionskontrollsystem_
* **GitHub**\
  _Git online Plattform_
  * **GitHub Actions**\
  _Automatisierungen in GitHub_
  * **GitHub Pages**\
  _gratis Hosting für statische Webseiten_
* **Node.js**\
  _JavaScript Runtime, um JavaScript auch ausserhalb des Browsers einsetzen zu können_
* **React**\
  _JavaScript Bibliothek für Benutzerschnittstellen_
* **Typescript**\
  _JavaScript + statische Typisierung für sicheren Code_
* **Docker**\
  _Virtualisierung_

## Voraussetzungen

Die diversen Softwarepakete welche eingesetzt werden, werden im Verlauf des Tutorials installiert. Wer das im voraus machen möchte, folgende Software wird eingesetzt:

* Git (https://git-scm.com/downloads)
* Node.js (https://nodejs.org)
* Docker (https://www.docker.com/get-started)

Alle eingesetzte Software sind plattformunabhängig und funktionieren unter Windows, Linux und macOS. 

**Ich persönlich empfehle, Linux zu verwenden. Insbesondere Docker läuft unter Linux wesentlich besser als unter Windows oder macOS.**

## Git installieren

### Windows

Auf https://git-scm.com/downloads den Installer herunterladen und alles installieren (die Default Einstellungen sind in Ordnung).

Nebst Git wird auch die **Git Bash** installiert und ich empfehle diese Konsole für die weiteren Schritte zu verwenden. Die Git Bash ist Linux-Kompatibel. Mir sind Befehle in Zusammenhang mit Node.js bekannt, welche in der Vergangenheit Probleme gemacht wenn die PowerShell oder CMD verwendet wird.

Ausserdem sind die Befehle, welche später im Zusammenhang mit Docker verwendet werden ebenfalls Bash-Shell kompatibel. Von daher macht es sowieso Sinn, sich damit vertraut zu machen.

### Linux (Debian/Ubuntu)

```console
$ sudo apt install git
```

## Node.js installieren

### Windows

Auf https://nodejs.org den Installer herunterladen und alles installieren (die Default Einstellungen sind in Ordnung).

Nachher ausloggen oder den Computer neustarten. Erst nachher wird die Umgebungsvariable `PATH` neu geladen, welche im Installationsvorgang (bei Git und Node.js) angepasst wird.

### Linux (Debian/Ubuntu)

Der Anleitung auf https://nodejs.org/en/download/package-manager/ befolgen.

Für Debian/Ubuntu

```console
$ curl -fsSL https://deb.nodesource.com/setup_14.x | sudo -E bash -
$ sudo apt-get install -y nodejs
```

## React/Typescript Projekt initialisieren

_Das folgende ist eine Zusammenfassung des Tutorials unter https://create-react-app.dev/docs/adding-typescript/_

```console
$ cd /path/to/project/folder
$ npx create-react-app vier-gewinnt --template typescript
$ cd vier-gewinnt
$ git log
commit 52f4b0f31d459b88186817f35dfa0940b2864e5a (HEAD -> master)
Author: Adrian Imboden <mailaddress@example.com>
Date:   Wed Feb 17 13:13:44 2021 +0000

    Initialize project using Create React App
```

Jetzt existiert ein leeres Typescript/React.js Projekt. Der `git log` Befehl zeigt, dass unser Git Repository einen Commit drin hat.

## Git Konfigurieren

Jetzt auf https://github.com registrieren. 

Üblicherweise verwendet man Git über SSH. Für die Authentifizierung wird ein SSH-Schlüsselpaar benötigt. Dieses wird wie folgt erstellt:

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

Zum Ausprobieren ob alles funktioniert hat, kann man dieses Respository klonen:

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

Das erste mal muss man den github.com Endpunkt bestätigen. Falls später mal eine Man-in-the-middle stattfinden würde, dann würde Git/SSH das bemerken.

## Git Repository hochladen

Jetzt auf https://github.com ein neues Repository (kurz: Repo) erstellen.

Für dieses Tutorial wurde https://github.com/adrianimboden/vier_gewinnt gewählt.

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

Commit im Beispiel: https://github.com/adrianimboden/vier_gewinnt/commit/52f4b0f31d459b88186817f35dfa0940b2864e5a

## Build und Tests lokal ausführen

Damit lokal und auf dem Build-Server die gleiche Umgebung eingesetzt wird, bietet sich hier Docker an. Bevor das ganze automatisiert wird, kann man es mal lokal ausführen:

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

Jetzt ist im Ordner `build` die statische Webseite, welche wir auf einen beliebigen Webserver hochladen könnten und die Tests sind erfolgreich durchgelaufen.

## Build und Tests mit Docker ausführen

Jetzt da alles funktioniert, werden wir das ganze mittels Docker durchführen.

Docker kann man als leichtgewichtige virtuelle Maschine ansehen. Ein Dockerfile beinhaltet den Ablauf aus dem vorherigen Kapitel.

Eine Datei mit dem Namen "Dockerfile" (keine Dateiendung) anlegen. Folgender Inhalt:

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

Das Dockerfile kann jetzt folgendermassen ausgeführt werden:

```console
$ cd /path/to/project/folder/vier-gewinnt
$ docker build .
```

Das Dockerfile im Detail:

Befehl | Beschreibung
------------------------------------------------- | -------------
| `FROM ubuntu:focal`                             | Als Basis ein fertig installiertes Ubuntu in der Version focal nehmen |
| `RUN apt-get update && apt-get install -y curl` | curl installieren (wird für die Installation von Node.js verwendet |
| ```RUN curl -fsSL https://deb.nodesource.com/setup_14.x \| bash -```<br>`RUN apt-get install -y nodejs` | Node.js intallieren (das ist der Befehl vom Tutorial am Anfang) |
| `WORKDIR /src` | |In den Pfad /src wechseln (ist das selbe wie `cd /src`, aber das würde im Dockerfile nur für ein einzelnes RUN gelten) |
| `COPY package.json /src/package.json`<br>`COPY package-lock.json /src/package-lock.json` | Für das installieren der Projekt-Dependencies wird nur package.json und package-lock.json verwendet. Man könnte auch direkt auf dem ganzen Source arbeiten, das würde dann aber bedeuten dass bei jeder kleinsten Code-Änderung immer wieder alle Abhängigkeiten von Null installiert werden müssen. |
| `RUN npm install` | Installieren der Projektabhängigkeiten. Das wurde im Tutorial automatisch von `npx create-react-app` gemacht. |
| `COPY . /src/` | Den kompletten Source-Code in die Docker-Umgebung kopieren. |
| `RUN npm run-script build`<br>`RUN CI=true npm test` | Der eigentliche Build- und Testablauf |

## node_modules

Wer vorher genau hingeschaut hat, hat vielleicht bemerkt dass beim Befehl `docker build .` ca. 250MiB an Daten an den Docker Daemon gesendet werden. Das ist viel mehr, als der Source-Code gross ist. Der Grund ist, dass alle Projektabhängigkeiten (der `node_modules`-Ordner) mit hineinkopiert wird. Dieser ist aber nicht Teil des Git Repositories und wird auf dem Buildserver auch nicht vorhanden sein.

Deshalb noch eine Datei mit dem Namen `.dockerignore` (analog zu .gitignore) erstellen. Folgender Inhalt:

```
node_modules/**
```

Der Befehl `RUN npm install` erstellt diesen Ordner dann als Teil der Dependency-Installation.


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

Einige der häufigsten Git Kommandos:

| Befehl | Beschreibung |
| ------ | ------------ |
| `git status` | Anzeigen, was für Dateien wir seit letztem mal editiert/hinzugefügt/gelöscht haben |
| `git add` | Geänderte, Gelöschte und Hinzugefügte Dateien "stagen" |
| `git diff --staged` | Die Differenz zwischen dem neusten Commit (HEAD) und dem "stage"-Bereich anzeigen |
| `git commit -m"Beschreibung"` | Den "stage"-Bereich mit einer Beschreibung abspeichern |
| `git push` | Die lokalen Commits welche noch nicht auf dem Server sind hochladen |

Ein gutes Tutorial, für jene welche GIT verstehen möchten: https://www.sbf5.com/~cduan/technical/git/

Commit im Beispiel: https://github.com/adrianimboden/vier_gewinnt/commit/c19e869ecba8ef4505103841c049fc3c3d5c9a96

## GitHub Action einrichten

GitHub Actions erstellen, welche bei jeder Änderung das Dockerfile builden. Das ist der erste Schritt für ein sauberes CI.

Folgende Datei und Ordnerstruktur im Repository erstellen:

`.github/workflows/ci.yaml`

mit folgendem Inhalt:
```YAML
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

Der Job selber läuft auf einer Ubuntu 20.04 Maschine, welche von GitHub gestellt wird.

Das ganze jetzt wieder wie vorher beschrieben auf Github hochladen (`git add .`, `git commit -m"..."`, `git push`), danach kann die laufende Action auf GitHub betrachtet werden:

![GitHub Action in Action](github_action.png)

Der orange Kreis zeigt, dass die Action noch am laufen ist.

Commit im Beispiel: https://github.com/adrianimboden/vier_gewinnt/commit/820be9657ee833b2dbe6825cb04b557d5a9f2e18

## Deployment

Da das Projekt (bis jetzt) eine reine Frontend-Applikation ist, kann sie mittels GitHub Pages gehostet werden. Dazu braucht es ein weiteres Repository. Im Tutorial wird https://github.com/adrianimboden/vier-gewinnt-deploy dafür verwendet.

Wie beim Git Einrichten wird ein SSH-Keypair für das Hochladen des Build-Resultats auf das Deploy repository benötigt:

```console
$ ssh-keygen -f tmp
... output omitted ...
$ cat tmp.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDMfO9dLLbane/oGA+AtuEJUHBwZMtjx4tar0PvuYLCID6STh2i0NAG5vMXe0+sPUCnt7pZ00fQQ3hFnNWgDV7WBxUqowNnVaS5teDVF0j2Glp9GxSXWOE6UdVyAeMntPxgAjhC9evoEyX8Gfs3BgWapkdvG7/nI4/hglxtncEFJ54dKWyzax/ZiFFwMTha69Zi9CYcC8ANeYWmankDTPvR08rFHl2YPXe+NCERDqPKmuoBvLQxi6ct2u7me0Jw8sxu4tJzZGdS2c2IO54/P2Q9fcR+9CRbPdAotU5MyeOhM2I2MbhezquREr+uJJg8BU57HkBvNjF+Qy/cn6hTEcy8/pWrkYtoBmGcvUOTA8kKU4mZJYPNXnSh12jGaBBCBEidaLp/1Z8rgjITzkRzTlvfTr/4M8zbqS5wizjh4qAu9ilmcMDnEB0ZDCFo4yBiLZLv/AJActNrYNFvWonI3KSlems64tLZ9+ZDUPSy5OYjfWwkS9qu8ZbYc5OXDbv9700= user@computer
$ cat tmp
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAABlwAAAAdzc2gtcn
NhAAAAAwEAAQAAAYEAzHzvXSy22p3v6BgPgLbhCVBwcGTLY8eLWq9D77mCwiA+kk4dotDQ
BubzF3tPrD1Ap7e6WdNH0EN4RZzVoA1e1gcVKqMDZ1WkubXg1RdI9hpafRsUl1jhOlHVcg
HjJ7T8YAI4QvXr6BMl/Bn7NwYFmqZHbxu/5yOP4YJcbZ3BBSeeHSlss2sf2YhRcDE4WuvW
YvQmHAvADXmFpmp5A0z70dPKxR5dmD13vjQhEQ6jyprqAby0MYunLdru5ntCcPLMbuLSc2
RnUtnNiDuePz9kPX3EfvQkWz3QKLVOTMnjoTNiNjG4Xs6rkRK/riSYPAVOex5AbzYxfkMv
3J+oUxHMvP6Vq5GLaAZhnL1DkwPJClOJmSWDzV50oddoxmgQQgRInWi6f9WfK4IyE85Ec0
5b306/+DPM26kucIs44eKgLvYpZnDA5xAdGQwhaOMgYi2S7/wCQHLTa2DRb1qJyNykpXpr
OuLS2ffmQ1D0suTmI31sJEvarvGW2HOTlw27/e9NAAAFiN+/MKHfvzChAAAAB3NzaC1yc2
EAAAGBAMx8710sttqd7+gYD4C24QlQcHBky2PHi1qvQ++5gsIgPpJOHaLQ0Abm8xd7T6w9
QKe3ulnTR9BDeEWc1aANXtYHFSqjA2dVpLm14NUXSPYaWn0bFJdY4TpR1XIB4ye0/GACOE
L16+gTJfwZ+zcGBZqmR28bv+cjj+GCXG2dwQUnnh0pbLNrH9mIUXAxOFrr1mL0JhwLwA15
haZqeQNM+9HTysUeXZg9d740IREOo8qa6gG8tDGLpy3a7uZ7QnDyzG7i0nNkZ1LZzYg7nj
8/ZD19xH70JFs90Ci1TkzJ46EzYjYxuF7Oq5ESv64kmDwFTnseQG82MX5DL9yfqFMRzLz+
lauRi2gGYZy9Q5MDyQpTiZklg81edKHXaMZoEEIESJ1oun/VnyuCMhPORHNOW99Ov/gzzN
upLnCLOOHioC72KWZwwOcQHRkMIWjjIGItku/8AkBy02tg0W9aicjcpKV6azri0tn35kNQ
9LLk5iN9bCRL2q7xlthzk5cNu/3vTQAAAAMBAAEAAAGAey3PKrtisrJs78x+mjSPvoKcGL
g+5K4xPcFSFTgUkkfGKW+w3oEWrvG0AbMgPUrwhZdhQ/bHzaa27ogZDz6+oALIN1TXGgo3
Hj93XiWWp9kcJTyI3NqHYhAlmXWVljepx1PO/FcTcksGmKQ19eFEwvHjGgUHRn7fjyJt9w
IOYcKJtltbxYCEQQ+irwiXP/3MuPMFeeOCHxvS7Ho6x0g6cR05iiBOlQSB+yt/t7yv4b4L
tFwt16KeZDr7qg8Z6Nv4QgCHzOsdQmC7ddAgFuVp03Ep3oAFKLX5Bar0s+G9PEDw1ljjjs
6jw4eR3aUeRCzADDveZZ/SQUdVbqBvPHmAsuH5XLkIHJkc57fsGgC5243gqijLkMgaVi5P
q/jKXbexp+CXv2LKrb0JmY9Po1YV8RIHsZ55XpUpEzeWcR1tIBF1IVBv0/v+JQqQd41ZQ9
2JBc5GaBMetuB8Q63yN9cyWy4IUnaibXy6iI7qj5wY+a/BlGPyKSFACCHO1IFNvXSBAAAA
wQCylJrj3IRmMeB0pV6mKHfZ92s3Nz2und8LsrQMy1BYVWlyDnfeHcDJe/tjtqi5CrZDnS
ZNU4KbmtTqZmhhDUMQtyyF2zf6m5gFbITFHQzMOHRtj6hBFKr2dBaiLEZZ5OA+3TEXPnJc
DYDX526DdkbrsXaInQpcMZGzKS0e2BdZzBa7FJu9D3MS1zjRSilVwrOk4xQ8TFuZQSilA6
tROAGKyv/n26qD3Kudq6gvBeqPc1uNkqeD/OSaoFdBkiCldoAAAADBAO3LGriCGc50K5qx
h/RSoeRIgsmd+B/WCZw4DWe3nXDvkqFDpCjRsO+2kWDrNdCmzSB1rLfdN3Sx/XlvMwmVgX
EKTSnhWWPVaxkjnTD2PpilQ233b+zTMtv9wMayM8iTm3vbchLY+nKmg8balMe+P+nbD5Mm
w8LnWV6EAvT7gDVi7NiYKOjl4aOnZLAgbwQAZcVUsGrgSXL7WuuV8DbTqgmb+8+u610QQK
XlimEVjYQWb/y01FWrZMwfmy1jxiqMPQAAAMEA3CUGgkEbWvx6mgHVMYIiYVVVhwzSJ+e+
sRawtdJQyNIuIde4d+jcKhlkpdzS0MTlZH1fUt+V18FE67ep2HQnoPaaZyM9e8x3q18boC
GeO7r7h8ivk2EMOJJuGjDLZ08Jwp2JKxl/5iK3vQNlrSjxircV1FoXp4GZC9m6zcO3oKrL
PJwIa6cMJyvexzjw721HFVzV/RfBAwX51xtkCObroH8TWMsOGLkPrpt/frSI8o5t4YvNhg
JLQFyWs01sTtBRAAAADWFkcmlhbkBhZGlkZXYBAgMEBQ==
-----END OPENSSH PRIVATE KEY-----
```

Den Public Key (tmp.pub) im Deploy-Repository unter `Settings/Deploy Keys` hinzufügen (mit Write-Access).

Den Private Key (tmp) im Source-Repository unter `Settings/Secrets` hinzufügen (im Tutorial wird `DEPLOY_KEY` als Name verwendet)

Für das Deployment golgende Datei im Repository erstellen: `.github/workflows/cd.yaml`

mit folgendem Inhalt:
```YAML
name: Deploy

on:
  push:
    branches:
      - master


jobs:
  build:
    runs-on: ubuntu-20.04

    steps:
      - uses: actions/checkout@v2
      - run: docker build -t build_tmp .
      - run: docker create -ti --name dummy build_tmp bash
      - run: docker cp dummy:/src/build build
      - run: docker rm -f dummy
        
      - uses: peaceiris/actions-gh-pages@v3  
        with:
          deploy_key: ${{ secrets.DEPLOY_KEY }}
          external_repository: adrianimboden/vier-gewinnt-deploy 
          publish_branch: master
          publish_dir: build/
```

Der Hauptunterschied zum normale CI Build ist, dass nach dem `docker build` Ablauf das Buildresultat aus dem Image entnommen wird.

Nachher wird das Buildresultat mit dem Deploy-Key, welcher in Repository-Konfiguration gespeichert ist, auf das Deploy Repository hochgeladen.

Es gibt 3 Arten, wie die GitHub Page angesehen werden kann:
- Eine einzige GitHub Page pro User: https://username.github.io für https://github.com/username/vier-gewinnt-deploy
- GitHub URL pro Repository: https://username.github.io/vier-gewinnt-deploy für https://github.com/username/vier-gewinnt-deploy
- Eine separate Domain, welche von extern gestellt wird: z.B. http://vier-gewinnt.example für https://github.com/username/vier-gewinnt-deploy

Bei den ersten beiden Varianten ist die aktuelle Konfiguration bereits in Ordnung, weil das React-Template darauf ausgelegt ist, dass die Applikation im Root Deployed wird.

Im Beispiel dieses Tutorials wird eine URL pro Repository verwendet, das heisst das Deployment geht hierhin: https://github.com/adrianimboden/vier-gewinnt-deploy.
Damit die Applikation dann noch funktioniert, muss in der Datei package.json der Eintrag `"homepage": "vier-gewinnt-deploy/"` hinzugefügt werden.

Commit im Beispiel: https://github.com/adrianimboden/vier_gewinnt/commit/8e97484e03a567447205fe790d32e466d70533f1
