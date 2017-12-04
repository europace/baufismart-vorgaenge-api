# Vorgänge Auslesen API

##### Aktuelle Version: 2.1

API Definition zum Auslesen von Vorgängen aus der Europace-Plattform aus Sicht eines Vertriebes.

### Swagger Spezifikationen
Die API ist vollständig in Swagger definiert. Die Swagger Definitionen werden sowohl im JSON- ([swagger.json](swagger.json)) als auch im YAML-Format ([swagger.yaml](swagger.yaml)) zur Verfügung gestellt.

Diese Spezifikationen können auch zur Generierung von Clients für diese API verwendet
werden. Dazu empfehlen wir das Tool [Swagger Codegen](https://github.com/swagger-api/swagger-codegen)

### Dokumentation

- [RELEASE NOTES](https://github.com/hypoport/vorgaenge-api/releases)
- [statische HTML Seite](http://htmlpreview.github.io?https://raw.githubusercontent.com/hypoport/vorgaenge-api/master/Dokumentation/index.html)

### Generierung des Clients
##### JAVA mit Retrofit

1. Die aktuelle Swagger Version, mindestens 2.2.2, downloaden
2. Client mit folgendem Kommando generieren:

Example:
```
java -jar swagger-codegen-cli-2.2.2.jar generate -i swagger.yaml -l java -c codegen-config-file.json -o europace-api-client
```

Example **codegen-config-file.json**:

```
{
  "artifactId": "europace-api-client",
  "groupId": "de.europace.api",
  "library": "retrofit2",
  "artifactVersion": "0.1",
  "dateLibrary": "java8"
}

```

### Authentifizierung

Die Authentifizierung läuft über den [OAuth2](https://oauth.net/2/) Flow vom Typ *ressource owner password credentials flow*.
https://tools.ietf.org/html/rfc6749#section-1.3.3

##### Credentials
Um die Credentials zu erhalten, erfagen Sie beim Helpdesk der Plattform die Zugangsdaten zur Auslesen API, bzw. bitten Ihren Auftraggeber dies zu tun.

##### Schritte 
1. Absenden eines POST Requests auf den [Login-Endpunkt](https://htmlpreview.github.io/?https://raw.githubusercontent.com/hypoport/vorgaenge-api/master/Dokumentation/index.html#_oauth2) mit Username und Password. Der Username entspricht der PartnerId und das Password ist der API-Key. Auf dem Testsystem können diese Werte frei gewählt werden. Alternativ kann ein Login auch über einen GET Aufruf mit HTTP Basic Auth auf den Login-Endpunkt erfolgen.
2. Aus der JSON-Antwort das JWToken (access_token) entnehmen
3. Bei weiteren Requests muss dieses JWToken als Authorization Header mitgeschickt werden.

### Quickstart

##### Den Status eines Vorgangs setzen

Der Status eines Vorgangs kann mittels eines PATCH-Requests auf https://baufismart.api.europace.de/v1/vorgaenge/{vorgangsNummer} mit folgendem Body geändert werden:
```
[
	{
		"op": "replace",
		"path": "/status",
		"value": "AKTIV"
	}
]
```

Als value sind hier AKTIV und ARCHIVIERT erlaubt.

##### Den Kundenbetreuer und/oder den Bearbeiter eines Vorgangs setzen
Der Kundenbetreuer und der Bearbeiter eines Vorgangs sind als separate Ressourcen mittels GET-Request auf https://baufismart.api.europace.de/v2/vorgaenge/{vorgangsNummer}/kundenBetreuer bzw. https://baufismart.api.europace.de/v2/vorgaenge/{vorgangsNummer}/vorgangsBearbeiter abrufbar. Ein PUT-Request mit folgendem Body auf diese Ressourcen setzten den jeweiligen Wert neu:
```
{
	"partnerId": "OEJ16"
}
```

Hierbei muss im Feld `partnerId` eine gültige PartnerID unserer Plattform übergeben werden. Des Weiteren muss der entsprechende Partner auch die Berechtigungen haben den Vorgang zu übernehmen.

Der Kundenbetreuer sowie der Bearbeiter eines Vorgangs können auch über PATCH-Requests auf der jeweiligen Ressource mit folgendem Body neu gesetzt werden:
```
[
	{
		"op": "replace",
		"path": "/partnerId",
		"value": "OEJ16"
	}
]
```
