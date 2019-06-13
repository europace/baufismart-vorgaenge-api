# Vorgänge API

API Definition zum Auslesen von Vorgängen aus der Europace-Plattform aus Sicht eines Vertriebes. 2 Felder können auch geschrieben werden: Kundenbetreuer und Vorgangsstatus.

# Dokumentation

*Aktuelle Version: 2.7*

### Swagger Spezifikationen
Die API ist vollständig in Swagger definiert. Die Swagger Definitionen werden sowohl im JSON- ([swagger.json](swagger.json)) als auch im YAML-Format ([swagger.yaml](swagger.yaml)) zur Verfügung gestellt.

Diese Spezifikationen können auch zur Generierung von Clients für diese API verwendet
werden. Dazu empfehlen wir das Tool [Swagger Codegen](https://github.com/swagger-api/swagger-codegen)

### Dokumentation

- [RELEASE NOTES](https://github.com/europace/baufismart-vorgaenge-api/releases)
- [Vollständige Dokumentation als HTML Seite](http://htmlpreview.github.io?https://raw.githubusercontent.com/europace/baufismart-vorgaenge-api/master/dokumentation/index.html)
- [Patch Beispiele](https://github.com/europace/baufismart-vorgaenge-api/blob/master/dokumentation/patch.md)

### Generierung des Clients
##### JAVA mit Retrofit

1. Die aktuelle Swagger Version (mindestens 2.2.2) downloaden
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
1. Absenden eines POST Requests auf den [Login-Endpunkt](https://htmlpreview.github.io/?https://raw.githubusercontent.com/europace/baufismart-vorgaenge-api/master/dokumentation/index.html#_oauth2) mit Username und Password. Der Username entspricht der PartnerId und das Password ist der API-Key. 
2. Aus der JSON-Antwort das JWToken (access_token) entnehmen
3. Bei weiteren Requests muss dieses JWToken als Authorization Header mitgeschickt werden.
