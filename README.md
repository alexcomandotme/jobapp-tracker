# job-tracker

Local job application tracker with Google Sheets sync.

## usage

Open `job-tracker.html` in a browser. Data persists in localStorage.

## sheets sync

1. Create a Google Sheet and open Extensions → Apps Script
2. Paste this:

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

3. Deploy → New deployment → Web app → Execute as: Me → Access: Anyone
4. Copy the `/exec` URL into `job-tracker.html` at `SHEETS_URL`

## notes

- Links render correctly without `https://` prefix
- CSV export/import available for backup
- Green dot = synced, red dot = error
