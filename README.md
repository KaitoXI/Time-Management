# Praxisarbeit – HFSNT.H.6.11-BE-S2405-VCID.IA1A.PA

## Anleitung zum Starten der Applikation auf der lokalen Umgebung

### Starten mit Docker

Klonen Sie das Repository und navigieren Sie im Terminal bzw. Kommandozeile (Konsole) zum geklonten Repository. 
Starten Sie die Docker Services mit:
```shell
docker compose up -d
```
> Beachten Sie, dass die Applikation im Hintergrund gestartet wird und nicht direkt in der Konsole. Falls Sie bewusst die
> Applikation in der Konsole starten möchten, führen Sie den Befehl ohne `-d` aus: `docker compose up`.

Die Applikation kann folgendermassen wieder beendet werden:
```shell
docker compose down
```
> Falls Sie die Applikation ohne `-d` ausgeführt haben, drücken Sie CTRL + C auf Windows/Linux und CMD + C auf MacOS.

### Starten auf Windows

Falls Sie die Applikation ohne Docker-Unterstützung starten möchten, führen Sie die folgenden Befehle in der Kommandozeile aus. 
Achten Sie dabei darauf, dass Sie sich in der Konsole im Ordner des geklonten Repositorys befinden.
Es wird davon ausgegangen, dass Sie [Python Version 3](https://www.python.org/downloads/) & [virtualenv](https://pypi.org/project/virtualenv/) installiert haben.
```shell
virtualenv venv
.\venv\Scripts\activate.bat # für Powershell bitte .\venv\Scripts\activate.ps1 ausführen
pip3 install -r requirements.txt

flask db init
flask db migrate -m "Initialize database"
flask db upgrade

set FLASK_APP=zeitverwaltung.py
flask run
```

### Starten auf MacOS oder Linux

Zum starten der Applikation, führen Sie folgende Schritte im Terminal durch. Vergewissern Sie sich, dass Sie sich in der Konsole 
im geklonten Repository-Ordner befinden. Es wird davon ausgegangen, dass Sie [Python Version 3](https://www.python.org/downloads/) & [virtualenv](https://pypi.org/project/virtualenv/) installiert haben.
```shell
virtualenv venv
source ./venv/bin/activate
pip3 install -r requirements.txt

flask db init
flask db migrate -m "Initialize database"
flask db upgrade

export FLASK_APP=zeitverwaltung.py
flask run
```

## Allgemeine Info

Dieses Repository beinhaltet eine einfache Webapplikation programmiert in Python Flask mit einer Anbindung an eine 
relationale Datenbank MariaDB. Die Webapplikation wurde von mir selbst programmiert. Gewisse Teile der Applikation wurden 
vom [Microblog](https://github.com/miguelgrinberg/microblog) von Miguel Grinberg übernommen. Es betrifft hauptsächlich 
die Struktur der Dateien und der Registrierung- & Login-Mechanismus.

### Was ist Time Management?

Time Management ist eine benutzerfreundliche Webapplikation, die es Nutzern ermöglicht, ihre geleisteten Arbeitsstunden einfach zu erfassen.  
Nach der Registrierung und dem Einloggen haben die Nutzer *ausschliesslich* Zugriff auf ihre eigenen Daten. 
Für jeden Tag kann maximal *ein Eintrag* erfasst werden, in dem die geleisteten Stunden angegeben werden. 
Optional kann auch für jeden Eintrag ein Kommentar erfasst werden. Die erfassten Stunden lassen sich einsehen, 
bearbeiten oder wenn nötig löschen. In der Profilübersicht sind allgemeine Informationen des Nutzers sichtbar. 
Neben den persönlichen Daten aus der Registrierung wird auch angezeigt, wie viele Arbeitsstunden insgesamt bereits geleistet wurden. 
Darüber hinaus können die Nutzer ihren aktuellen Stand bei Über- oder Minusstunden (Flextime) einsehen.

### Autor

Sinsakhone Phongsanith

### Erreichbarkeit im Internet

Die Webapplikation ist für eine geraume Zeit öffentlich im Internet erreichbar unter der URL [http://ec2-16-171-38-156.eu-north-1.compute.amazonaws.com:5000/](http://ec2-16-171-38-156.eu-north-1.compute.amazonaws.com:5000/).

### Repository vom Source Code

Der komplette Code befindet sich öffentlich zugänglich auf GitHub unter folgendem Repository: [https://github.com/KaitoXI/Time-Management](https://github.com/KaitoXI/Time-Management)

### Quellen

Teile der Applikation wurden inspiriert oder komplett von [MicroBlog](https://github.com/miguelgrinberg/microblog) entnommen.

## Technologien & Bibliotheken

Folgende Technologien kommen zum Einsatz in der Webanwendung:
- Programmiersprache: *[Python](https://www.python.org/)*
- Web-Framework: *[Flask](https://flask.palletsprojects.com/en/2.2.x/)*
- Datenbanksystem: *[MariaDB](https://mariadb.org/)*
- Containerisierung: *[Docker bzw. docker-compose](https://www.docker.com/)*
- Rendering der Webseiten: *[Jinja2](https://jinja.palletsprojects.com/en/3.1.x/)*
- Styling Bibliothek: *[Bootstrap](https://getbootstrap.com/)*
- Benutzte Icons: *[fontawesome](https://fontawesome.com/)*

## WebAPI

Die WebAPI ist eine Schnittstelle um HTTP Anfragen an den Webserver zu schicken und ggf. Daten zu laden. Die Webapplikation 
bietet derzeit APIs für folgende Szenarien:
- das Erstellen & Bearbeiten eines Nutzers
- Erstellen & Löschen eines Authentifizierung-Tokens
- Auslesen aller erfassten Arbeitsstunden
- Erstellen, Bearbeiten, Auslesen & Löschen erfasster Arbeitsstunden

Die Schnittstellen sind über den Pfad `/api/users`, `/api/working-hours` und `/api/tokens` erreichbar.

### Token API

#### Neuen Token erstellen

Base-Auth: `<USERNAME>:<PASSWORD>`
> Die Authentifizierungsdaten werden in der URL mitgegeben. Beispielsweise: `https://<USERNAME>:<PASSWORD>http://ec2-16-171-38-156.eu-north-1.compute.amazonaws.com:5000/api/tokens`

Pfad: `/api/tokens`

Methode: `POST`

Dieses Beispiel als `curl` Kommando:
```shell
curl http://TimeManagement:secret-12345@ec2-16-171-38-156.eu-north-1.compute.amazonaws.com:5000/api/tokens -X POST
```

#### Token wiederrufen bzw. löschen

Pfad: `/api/tokens`

Methode: `DELETE`

Authorization im Header: `Authorization: Bearer <TOKEN>`
> Ersetzen Sie `<TOKEN>` durch Ihren eigenen Token.

Dieses Beispiel als `curl` Kommando:
```shell
curl http://ec2-16-171-38-156.eu-north-1.compute.amazonaws.com:5000/api/tokens -X DELETE -H "Authorization: Bearer 62zNTEL/texJjlRyH5wo5KbtYDuUwvUM"
```


### Users API

#### Erstellen eines neuen Nutzers

Pfad: `/api/users`

Methode: `POST`

Content-Type im Header: `Content-Type: application/json`

Body als JSON:
```json
{
  "username": "Tony",
  "email": "tony@test.com",
  "password": "secret-12345",
  "company": "CSAG",
  "job": "Engineer",
  "target_time": 8
}
```
> Ersetzen Sie die Beispielwerte im JSON mit Ihren eigenen.

Dieses Beispiel als `curl` Kommando:
```shell
curl http://ec2-16-171-38-156.eu-north-1.compute.amazonaws.com:5000/api/users -X POST -H "Content-Type: application/json" -d "{\"username\": \"Tony\", \"email\": \"tony@test.com\", \"password\": \"12345\", \"company\": \"CSAG\", \"job\": \"WebDev\", \"target_time\": 8}"
```

#### Auslesen des Nutzer-Profils

Pfad: `/api/users`

Methode: `GET`

Authorization im Header: `Authorization: Bearer <TOKEN>`
> Ersetzen Sie `<TOKEN>` durch Ihren eigenen Token.

Dieses Beispiel als `curl` Kommando:
```shell
curl http://ec2-16-171-38-156.eu-north-1.compute.amazonaws.com:5000/api/users -H "Authorization: Bearer 62zNTEL/texJjlRyH5wo5KbtYDuUwvUM"
```

#### Bearbeiten des Nutzer-Profils

Pfad: `/api/users`

Methode: `PUT`

Content-Type im Header: `Content-Type: application/json`

Authorization im Header: `Authorization: Bearer <TOKEN>`
> Ersetzen Sie `<TOKEN>` durch Ihren eigenen Token.

Body als JSON:
```json
{
  "username": "Tony",
  "email": "tony@test.com",
  "password": "secret-12345",
  "company": "CSAG",
  "job": "Engineer",
  "target_time": 8
}
```
> Ersetzen Sie die Beispielwerte im JSON mit Ihren eigenen. Es müssen nicht alle Werte mitgegeben werden, nur jene, 
> welche geändert werden sollen.

Dieses Beispiel als `curl` Kommando:
```shell
curl http://ec2-16-171-38-156.eu-north-1.compute.amazonaws.com:5000/api/users -X PUT -H "Authorization: Bearer 62zNTEL/texJjlRyH5wo5KbtYDuUwvUM" -H "Content-Type: application/json" -d "{\"company\":\"CSAG\", \"job\":\"Engineer\"}"
```

### Arbeitsstunden API

#### Erfassen neuer Arbeitsstunden

Pfad: `/api/working-hours`

Methode: `POST`

Content-Type im Header: `Content-Type: application/json`

Authorization im Header: `Authorization: Bearer <TOKEN>`
> Ersetzen Sie `<TOKEN>` durch Ihren eigenen Token.

Body als JSON:
```json
{
  "date": "2024-09-02",
  "working_hours": 8.4,
  "comment": "Time Management Flask Applikation"
}
```
> Ersetzen Sie die Beispielwerte im JSON mit Ihren eigenen.
> Beachten Sie, dass das Datum im Format YYYY-MM-DD mitgegeben werden muss.

Dieses Beispiel als `curl` Kommando:
```shell
curl http://ec2-16-171-38-156.eu-north-1.compute.amazonaws.com:5000/api/working-hours -X POST -H "Authorization: Bearer 62zNTEL/texJjlRyH5wo5KbtYDuUwvUM" -H "Content-Type: application/json" -d "{\"date\":\"2024-09-02\",\"working_hours\":\"8.4\",\"comment\":\"Time Management Flask Applikation\"}"
```

#### Auslesen aller bereits erfassen Arbeitsstunden

Pfad: `/api/working-hours`

Methode: `GET`

Authorization im Header: `Authorization: Bearer <TOKEN>`
> Ersetzen Sie `<TOKEN>` durch Ihren eigenen Token.

Dieses Beispiel als `curl` Kommando:
```shell
curl http://ec2-16-171-38-156.eu-north-1.compute.amazonaws.com:5000/api/working-hours -H "Authorization: Bearer 62zNTEL/texJjlRyH5wo5KbtYDuUwvUM"
```

#### Auslesen einer bestimmten erfassten Arbeitsstunden mit dem ID

Pfad: `/api/working-hours/<ID>`
> Hier `<ID>` durch die ID der erfassten Arbeitsstunden ersetzen. Diese ID kann man herausfinden, indem man alle Einträge der 
> Arbeitsstunden lädt und die ID ausliest.

Methode: `GET`

Authorization im Header: `Authorization: Bearer <TOKEN>`
> Ersetzen Sie `<TOKEN>` durch Ihren eigenen Token.

Dieses Beispiel als `curl` Kommando:
```shell
curl http://ec2-16-171-38-156.eu-north-1.compute.amazonaws.com:5000/api/working-hours/1 -H "Authorization: Bearer 62zNTEL/texJjlRyH5wo5KbtYDuUwvUM"
```

#### Bearbeiten einer bestimmten erfassten Arbeitsstunden

Pfad: `/api/working-hours/<ID>`
> Hier `<ID>` durch die ID der erfassten Arbeitsstunden ersetzen. Diese ID kann man herausfinden, indem man alle Einträge der
> Arbeitsstunden lädt und die ID ausliest.

Methode: `PUT`

Content-Type im Header: `Content-Type: application/json`

Authorization im Header: `Authorization: Bearer <TOKEN>`
> Ersetzen Sie `<TOKEN>` durch Ihren eigenen Token.

Body als JSON:
```json
{
  "working_hours": 9,
  "comment": "Time Management Dokumentation"
}
```
> Ersetzen Sie die Beispielwerte im JSON mit Ihren eigenen. Es kann nur der Kommentar bzw. die geleistete Arbeitsstunden bearbeitet werden.

Dieses Beispiel als `curl` Kommando:
```shell
curl http://ec2-16-171-38-156.eu-north-1.compute.amazonaws.com:5000/api/working-hours/1 -X PUT -H "Authorization: Bearer 62zNTEL/texJjlRyH5wo5KbtYDuUwvUM" -H "Content-Type: application/json" -d "{\"working_hours\":9,\"comment\":\"Time Management Dokumentation\"}"
```

#### Löschen einer bestimmten erfassten Arbeitsstunden

Pfad: `/api/working-hours/<ID>`
> Hier `<ID>` durch die ID der erfassten Arbeitsstunden ersetzen. Diese ID kann man herausfinden, indem man alle Einträge der
> Arbeitsstunden lädt und die ID ausliest.

Methode: `DELETE`

Authorization im Header: `Authorization: Bearer <TOKEN>`
> Ersetzen Sie `<TOKEN>` durch Ihren eigenen Token.

Dieses Beispiel als `curl` Kommando:
```shell
curl http://ec2-16-171-38-156.eu-north-1.compute.amazonaws.com:5000/api/working-hours/1 -X DELETE -H "Authorization: Bearer 62zNTEL/texJjlRyH5wo5KbtYDuUwvUM"
```


## Checkliste mit den Anforderungen der Praxisarbeit

- [x] interaktive Weboberfläche (Benutzereingaben, -verarbeitungen & -aktionen)
- [x] Authentifizierung & Autorisierung mit Benutzernamen, E-Mail Adresse & Passwort
- [x] Datenspeicherung in einer relationaler Datenbank
- [x] Implementierung der Geschäftslogik
- [x] mind. 1 Lesezugriff über RESTful Web-API
- [x] Scripts zur automatisierten Bau & Inbetriebnahme der Webanwendung
