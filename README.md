# Vorgaenge-API
As an advisor, I can use Vorgaenge API to read all the data from my cases and get furthermore links to applications, loans, documents and events.

![advisor](https://img.shields.io/badge/-advisor-lightblue)
![mortgageLoan](https://img.shields.io/badge/-mortgageLoan-lightblue)

[![Authentication](https://img.shields.io/badge/Auth-OAuth2-green)](https://docs.api.europace.de/common/authentifizierung/authorization-api/)
[![GitHub release](https://img.shields.io/github/v/release/europace/baufismart-vorgaenge-api)](https://github.com/europace/baufismart-vorgaenge-api/releases)

[![Pattern](https://img.shields.io/badge/Pattern-Tolerant%20Reader-yellowgreen)](https://martinfowler.com/bliki/TolerantReader.html)

## Documentation
[![YAML](https://img.shields.io/badge/OAS-HTML_Doc-lightblue)](https://europace.github.io/baufismart-vorgaenge-api/reference/index.html)
[![YAML](https://img.shields.io/badge/OAS-YAML-lightgrey)](https://raw.githubusercontent.com/europace/baufismart-vorgaenge-api/master/swagger.yaml)
[![YAML](https://img.shields.io/badge/OAS-JSON-lightgrey)](https://raw.githubusercontent.com/europace/baufismart-vorgaenge-api/master/swagger.json)

## Usecases
- get data to:
  - create own financing proposals with individual structure and design
  - keep own CRM system up to date 
  - create individual notifications
- set data to:
  - change advisor and editor to control the workload of employees
  - mark applications as ready-to-check
  - set your own case reference

## Quick Start
To test our APIs and your use cases as quickly as possible, we have created a [Postman Collection](https://docs.api.europace.de/common/quickstart/) for you.

### Authentication
Please use [![Authentication](https://img.shields.io/badge/Auth-OAuth2-green)](https://docs.api.europace.de/common/authentifizierung/authorization-api/) to get access to the APIs. The OAuth2 client requires the following scopes:

| Scope                               | API Use case                  |
|-------------------------------------|-------------------------------|
| `baufinanzierung:vorgang:lesen`     | to get case data              | 
| `baufinanzierung:echtgeschaeft`     | to use api in production mode |
| `baufinanzierung:vorgang:schreiben` | to update case data (eg role) |

## Get data
### Get case data
As advisor I can read out the data of the case, to create an individual financial proposal for a convincing sales story.

Requirements:
- authenticated as advisor, editor or sales organisation with access to the case

example-request:
``` http
GET /v3/vorgaenge/CH6407 HTTP/1.1
Host: api.europace2.de
Content-Type: application/json
Authorization: Bearer {{access-token}} 
```

example-response:
``` json
{
  "_links": { ... },
  "vorgangsNummer": "CH6407",
  "erstelltAm": "2022-02-16",
  "letztesEreignis": "2022-02-22T08:31:01.37Z",
  "status": "AKTIV",
  "bankverbindung": { ... },
  "haushalte": [ ... ],
  "finanzierungsObjekt": { ... },
  "vorhaben": { ... },
  "datenKontext": "TEST_MODUS",
  "aufbewahrungBis": "2025-02-27",
  "kundenBetreuer": { ... },
  "antraege": [ ... ],
  "vorgangsBearbeiter": { ... }
}
```
For the full model [see API-Specification](https://europace.github.io/baufismart-vorgaenge-api/docs/index.html#get-/v3/vorgaenge/-vorgangsNummer-)

### Get last changed cases

As advisor I will get a list of the last changed cases, to keep your CRM system up to date for a seamless, efficent and high quality sales process. The list of cases contains all cases where accessible for the caller and is descent ordered by lastchanged date and paged. 

Requirements:
- authenticated as advisor, editor or sales organisation with access to the cases

example-request:
``` http 
GET /v3/vorgaenge HTTP/1.1
Host: api.europace2.de
Content-Type: application/json
Authorization: Bearer {{access-token}}
```

example-response:
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
                    "href": "https://baufismart.api.europace.de/v3/vorgaenge/A74QK3"
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
                    "href": "https://baufismart.api.europace.de/v3/vorgaenge/ED7PIS"
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
> Pls note:
> the list is paged.

You can filter the results by using the following parameters:
- datenkontext (using test- or production-mode)
- aenderungSeit (lastChangeUntil for getting all changes after the last call)
- and many more - see documentation

## Set data

Changes to some metadata on a case are possible using [JSON Patch](http://jsonpatch.com/).
The path represents the path in the JSON model separated with /. The default operation supported is _replace_.
Patch operations are an array, so multiple patch commands can be sent with a _PATCH_ request.

### Set own reference

example-request:
```json
PATCH /v3/vorgaenge/CH6407 HTTP/1.1
Host: api.europace2.de
Content-Type: application/json-patch+json
Authorization: Bearer {{access-token}}
Content-Length: 75

[
	{"op":"replace","path":"/externeVorgangsNummer","value":"ext_VN_4711"}
]
```

example-response:
``` http
200 Okay
```
[see example-response of get case data](#get-case-data)

### Set state of case
As advisor you can set the state, to archive outdated cases and hide them for the advisors.
example-request:
``` json
PATCH /v3/vorgaenge/CH6407 HTTP/1.1
Host: api.europace2.de
Content-Type: application/json-patch+json
Authorization: Bearer {{access-token}}
Content-Length: 59

[
    {"op":"replace","path":"/status","value":"ARCHIVIERT"}
]
```

example-response:
``` http
200 Okay
```
[see example-response of get case data](#get-case-data)

Allowed values are `AKTIV` and `ARCHIVIERT`.

### Set advisor
As sales organsisation you can set the advisor to control the workload of the colleagues.

example-request:
```json
PUT /v3/vorgaenge/CH6407/kundenBetreuer HTTP/1.1
Host: api.europace2.de
Authorization: Bearer {{access-token}}
Content-Type: application/json
Content-Length: 26

{
"partnerId": "AAV43"
}
```

A valid partnerId of our platform must be transferred in the `partnerId` field. Furthermore, the corresponding partner need also the authorization to access the case.

example-response:
``` http
201 Created
```

### Set editor
As advisor you can set the editor eg to a clerk, to check the case and application or to a teammate to enrich the data of the case.

example-request:
```json
PUT /v3/vorgaenge/CH6407/vorgangsBearbeiter HTTP/1.1
Host: api.europace2.de
Authorization: Bearer {{access-token}}
Content-Type: application/json
Content-Length: 26

{
    "partnerId": "MNC81"
}
```
A valid partnerId of our platform must be transferred in the `partnerId` field. Furthermore, the corresponding partner need also the authorization to access the case.

example-response:
``` http
201 Created
```

## Rate-limiting
The API is rate limited to 2000 requests per minute per Client ID. If you exceed this limit you will get a http 429. 

## Terms of use
The APIs are provided under the following [Terms of Use](https://docs.api.europace.de/nutzungsbedingungen).

## Support
If you have any questions or problems, you can contact devsupport@europace2.de.
