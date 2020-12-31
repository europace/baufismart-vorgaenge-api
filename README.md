# Vorgaenge-API
Als Vermittler kann ich mit dieser API alle Daten aus meinen Vorgängen auslesen.

![Vertrieb](https://img.shields.io/badge/-Vertrieb-lightblue)
![Baufinanzierung](https://img.shields.io/badge/-Baufinanzierung-lightblue)

[![Authentication](https://img.shields.io/badge/Auth-OAuth2-green)](https://github.com/europace/authorization-api)
[![GitHub release](https://img.shields.io/github/v/release/europace/baufismart-vorgaenge-api)](https://github.com/europace/baufismart-vorgaenge-api/releases)

[![Pattern](https://img.shields.io/badge/Pattern-Tolerant%20Reader-yellowgreen)](https://martinfowler.com/bliki/TolerantReader.html)

## Dokumentation
[![YAML](https://img.shields.io/badge/OAS-HTML_Doc-lightblue)](https://europace.github.io/baufismart-vorgaenge-api/docs/index.html)
[![YAML](https://img.shields.io/badge/OAS-YAML-lightgrey)](https://raw.githubusercontent.com/europace/baufismart-vorgaenge-api/master/swagger.yaml)
[![YAML](https://img.shields.io/badge/OAS-JSON-lightgrey)](https://raw.githubusercontent.com/europace/baufismart-vorgaenge-api/master/swagger.json)

## Anwendungsfälle der API
- eigene Finanzierungsvorschläge mit individuellem Aufbau und Design erstellen
- eigenes CRM-System mit den BaufiSmart Daten aktualisieren
- Kundenbetreuer und Sachbearbeiter ändern und so die Auslastung der Mitarbeiter automatisch steuern
- individuelle Benachrichtigungen erzeugen
- Anträge als vollständig kennzeichnen

# Schnellstart
Damit du unsere APIs und deinen Anwendungsfall schnellstmöglich testen kannst, haben wir eine [Postman-Collection](https://docs.api.europace.de/baufinanzierung/schnellstart/) für dich zusammengestellt.

### Authentifizierung
Bitte benutze [![Authentication](https://img.shields.io/badge/Auth-OAuth2-green)](https://docs.api.europace.de/baufinanzierung/authentifizierung/), um Zugang zur API bekommen. Um die API verwenden zu können, benötigt der OAuth2-Client folgende Scopes:

| Scope                             | API Use case |
|-----------------------------------|---------------------------------|
| `baufinanzierung:vorgang:lesen`   | Daten des Vorgangs auslesen |
| `baufinanzierung:echtgeschaeft`   | nicht nur Testvorgänge abrufen, sondern produktive Vorgänge |


## Beispiel: Vorgang Auslesen

Für das Auslesen eines Vorgangs wird die sechs-stellige alpha-nummerische Vorgangsnummer (z.B. CH6407) als Referenz und ein gültiger Access-Token gebraucht. Der Access-Token muss Zugriffsrechte auf den Vorgang haben.

Request:
``` cURL
curl --location --request GET 'https://api.europace2.de/v2/vorgaenge/{{vorgangsnummer}}' \
--header 'Authorization: Bearer {{access_token}}'
```

Response:

[siehe API-Spezifikation](https://europace.github.io/baufismart-vorgaenge-api/docs/index.html#get-/v2/vorgaenge/-vorgangsNummer-)

## Beispiel: zuletzt geänderten Vorgänge ermitteln

Die API stellt eine Liste mit Vorgängen zur Verfügung, auf die der Access-Token Zugriff hat. Diese Liste ist absteigend sortiert nach der letzten Änderung. Damit wird ermöglicht, dass man zwischen zwei Zeitpunkten alle sich veränderten Vorgänge ermitteln und in einem weiteren Schritt auslesen kann.

Request:
``` cURL
curl --location --request GET 'https://api.europace2.de/v2/vorgaenge' \
--header 'Authorization: Bearer {{access_token}}'
```

Response:
``` JSON
{
    "vorgaenge": [
        {
            "datenKontext": "ECHT_GESCHAEFT",
            "vorgangsNummer": "A74QK3",
            "letztesEreignis": "2020-12-30T09:53:16.165Z",
            "letzteAenderung": "2020-12-30T09:53:16.126Z",
            "_links": {
                "self": {
                    "href": "https://baufismart.api.europace.de/v2/vorgaenge/A74QK3"
                }
            }
        },
        {
            "datenKontext": "TEST_MODUS",
            "vorgangsNummer": "ED7PIS",
            "letztesEreignis": "2020-12-30T09:52:34.557Z",
            "letzteAenderung": "2020-12-30T09:53:15.331Z",
            "_links": {
                "self": {
                    "href": "https://baufismart.api.europace.de/v2/vorgaenge/ED7PIS"
                }
            }
        },
        {
            "datenKontext": "ECHT_GESCHAEFT",
            "vorgangsNummer": "JA624A"
            ...
        }
    ]
}

```

## Vorgang verändern
[Erläuterungen & Patch-Beispiele](https://github.com/europace/baufismart-vorgaenge-api/blob/master/docs/patch.md)

## Nutzungsbedingungen
Die APIs werden unter folgenden [Nutzungsbedingungen](https://docs.api.europace.de/nutzungsbedingungen/) zur Verfügung gestellt.

## Support
Bei Fragen oder Problemen kannst du dich an devsupport@europace2.de wenden.
