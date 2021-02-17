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
`$ sudo apt install git`


## Node.js Installieren
### Windows:
Auf https://nodejs.org den Installer herunterladen und alles installieren (die Default Einstellungen sind in Ordnung).

Nachher ausloggen oder den Computer neustarten. Erst nachher wird die Umgebungsvariable PATH neu geladen, welche im Installationsvorgang (bei GIT und Node.js) angepasst wird.

### Linux (Debian/Ubuntu)
Der Anleitung auf https://nodejs.org/en/download/package-manager/ befolgen.
Für Debian/Ubuntu
```
$ curl -fsSL https://deb.nodesource.com/setup_14.x | sudo -E bash -
$ sudo apt-get install -y nodejs
```

## React/Typescript App initialisieren
Wer das ganze im Detail mit mehr Zusatzinfos machen möchte. Dieses Tutorial macht im Grunde das, was unter https://create-react-app.dev/docs/adding-typescript/ beschrieben ist.

```
$ npx create-react-app vier-gewinnt --template typescript
$ cd vier-gewinnt
$ git log
commit 52f4b0f31d459b88186817f35dfa0940b2864e5a (HEAD -> master)
Author: Adrian Imboden <adi@thingdust.com>
Date:   Wed Feb 17 13:13:44 2021 +0000

    Initialize project using Create React App
```

Jetzt haben wir ein leeres Typescript React.js Projekt. Der `git log` Befehl zeigt, dass unser GIT Repository einen Commit drin hat.
