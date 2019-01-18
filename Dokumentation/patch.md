## Änderung von Daten eines Vorgangs

Änderungen einiger Metadaten an einem Vorgang sind mittels [JSON Patch](http://jsonpatch.com/) möglich. 
Der Path repräsentiert den Pfad im JSON Modell mit / getrennt. Als Standardoperation wird _replace_ untertützt.
Patch-Operationen sind ein Array, es können also mehrere Patch Kommandos mit einer _PATCH_ Anfrage geschickt werden. 

### Änderbare Metadaten:

1. [Status des Vorgangs setzen](#Status-des-Vorgangs-setzen)
2. [Den Kundenbetreuer ändern](#Den-Kundenbetreuer-ändern)
3. [Den Vorgangsbearbeiter ändern](#Den-Vorgangsbearbeiter-ändern)
4. [Die eigene Vorgangsnummer ändern](#Die-eigene-\/-Vorgangsnummer-ändern)

### Status des Vorgangs setzen

Der Status eines Vorgangs kann mittels eines PATCH-Requests auf https://baufismart.api.europace.de/v1/vorgaenge/{vorgangsNummer} mit folgendem Body geändert werden:
```json
[
	{"op":"replace","path":"/status","value":"AKTIV"}
]
```

Als value sind hier AKTIV und ARCHIVIERT erlaubt.


### Den Kundenbetreuer ändern

Um den Kundenbetreuer zu Ändern muss nur die PartnerId des Kundenbetreuers gesetzt werden unter dem Pfad _/kundenBetreuer/partnerId_

```json
[
	{"op":"replace","path":"/kundenBetreuer/partnerId","value":"OEJ16"}
]
```

Hierbei muss im Feld `partnerId` eine gültige PartnerID unserer Plattform übergeben werden. Des Weiteren muss der entsprechende Partner auch die Berechtigungen haben den Vorgang zu übernehmen.

### Den Vorgangsbearbeiter ändern

Um den Vorgangsbearbeiter zu Ändern muss nur die PartnerId des Vorgangsbearbeiter gesetzt werden unter dem Pfad _/vorgangsBearbeiter/partnerId_

```json
[
	{"op":"replace","path":"/vorgangsBearbeiter/partnerId","value":"OEJ16"}
]
```

Hierbei muss im Feld `partnerId` eine gültige PartnerID unserer Plattform übergeben werden. Des Weiteren muss der entsprechende Partner auch die Berechtigungen haben den Vorgang zu übernehmen.

### Die eigene / externe Vorgangsnummer ändern

Im Feld _externeVorgangsNummer_ kann die eigene Vorgangsnummer gesetzt werden. Diese kann auch via Patch geändert werden:

```json
[
	{"op":"replace","path":"/externeVorgangsNummer","value":"ext_VN_4711"}
]
```
