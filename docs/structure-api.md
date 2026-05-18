# Struktur der Schnittstelle

Die Grundstruktur der JSON folgt der [JSON-RPC 2.0 Spezifikation](https://www.jsonrpc.org/specification).

Es muss im JSON-Format formuliert werden. Die Antwort entspricht ebenfalls dem JSON-Dateiformat. Die JSON wird im Body der HTTP Anfrage übermittelt.

Für die Übermittlung ist HTTPS (Port 443) zu setzen.

Aus Datenschutzgründen erlauben wir nur eine Server-to-Server-Kommunikation. Direkte Anfragen sind nicht erlaubt. So
vermeiden wir es, personenbezogene Daten des Endnutzers zu erhalten, die wir nicht brauchen.

Jedoch ist die Spezifikation nicht vollständig umgesetzt. Unter anderem gibt es aktuell keine Möglichkeit, mehrere
Datensätze (Bulk-Processing) zu übermitteln, und die Fehlercodes sind rudimentär und sollten in einer Integration nicht
verarbeitet werden.

### Request-Felder

| Feld | Erwarteter Wert | Bedeutung |
|------|----------------|-----------|
| `jsonrpc` | `"2.0"` | Version des Protokolls. Immer gleich. |
| `id` | Zahl > 0 | ID der Anfrage. Bei asynchronen Anfragen zur Sequenzwiederherstellung. |
| `method` | String | Name der Funktion, z.B. `addressCheck` |
| `params` | Object | Methodenspezifische Parameter |

### Response-Felder

| Feld | Typ | Bedeutung |
|------|-----|-----------|
| `result` | Object | Antwort bei erfolgreichem Verlauf |
| `error` | Object | Fehlermeldung bei fehlerhaftem Verlauf |
| `code` | Zahl | Fehlernummer. Aktuell rudimentär – soll ignoriert werden. |
| `message` | String | Fehlermeldung in menschenlesbarer Form |

## API-Endpunkt

Alle Anfragen werden als `POST` an folgenden Endpunkt gesendet:

```
POST https://endereco-service.de/rpc/v1
```

## Headers

| Header | Wert | Hinweis |
|--------|------|---------|
| `Content-Type` | `application/json` | |
| `X-Transaction-Id` | Session ID | Siehe [Session Guideline](guidelines/sessions-guideline.md) |
| `X-Agent` | z.B. `MyClient v1.0.0` | Siehe [Client ID Guideline](guidelines/client-id-guideline.md) |
| `X-Transaction-Referer` | z.B. `www.example.de/register` | Siehe [Referrer Guideline](guidelines/providing-referrer-guidlines.md) |
| `X-Auth-Key` | API-Key | Siehe [Authentifizierung](index.md#authentifizierung) |
