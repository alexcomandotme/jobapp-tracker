# Job Tracker

Tracker local pentru aplicații de job, cu sync automat în Google Sheets.

## Setup

1. Deschide `job-tracker.html` direct în browser
2. Datele se salvează în `localStorage` — rămân între sesiuni

## Google Sheets sync

**Apps Script** — codul care primește datele:

```javascript
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var data = JSON.parse(e.postData.contents);
  sheet.appendRow([data.company, data.role, data.status, data.link, data.note, data.date]);
  return ContentService
    .createTextOutput(JSON.stringify({ result: "ok" }))
    .setMimeType(ContentService.MimeType.JSON);
}
```

**Deployment:**
- Extensions → Apps Script → Deploy → New deployment
- Type: Web app
- Execute as: Me
- Who has access: Anyone
- Copiază URL-ul `/exec` și pune-l în `job-tracker.html` la `SHEETS_URL`

## Utilizare

| Câmp | Detalii |
|---|---|
| companie | numele companiei |
| rol | titlul poziției |
| link | URL job posting — `http://` se adaugă automat |
| status | de aplicat / aplicat / interviu / respins / fără răspuns |
| notiță | orice notă relevantă |

- **export CSV** — backup local
- **import CSV** — restaurare sau migrare (duplicate detection după companie + dată)
- Dot verde jos = sync
