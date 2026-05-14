# Apps Script Setup

รับ LINE webhook events และบันทึก userId ลง Google Sheets

> การส่ง LINE notification ดูได้ที่ [app-script-line-notify.md](app-script-line-notify.md)

---

## Web App URL

```text
YOUR_APPS_SCRIPT_WEB_APP_URL
```

## โค้ด (รหัส.gs)

```javascript
const SHEET_NAME = 'userId';

function doPost(e) {
  try {
    const body = JSON.parse(e.postData.contents);
    const events = body.events;
    if (!events || events.length === 0) return okResponse();

    const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName(SHEET_NAME);

    events.forEach(event => {
      const userId = event.source.userId;
      if (!userId) return;

      if (event.type === 'follow' || event.type === 'message') {
        if (!isUserExists(sheet, userId)) {
          sheet.appendRow([userId, new Date(), 'active']);
        } else {
          setStatus(sheet, userId, 'active');
        }
      } else if (event.type === 'unfollow') {
        setStatus(sheet, userId, 'inactive');
      }
    });

    formatSheet();
    return okResponse();
  } catch (err) {
    return okResponse();
  }
}

function doGet(e) {
  if (e.parameter.action === 'getActiveUsers') {
    const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName(SHEET_NAME);
    const data = sheet.getDataRange().getValues();
    const users = data
      .slice(1)
      .filter(row => row[2] === 'active')
      .map(row => row[0]);
    return ContentService.createTextOutput(JSON.stringify({ users }))
      .setMimeType(ContentService.MimeType.JSON);
  }
  return ContentService.createTextOutput(JSON.stringify({ status: 'ok' }))
    .setMimeType(ContentService.MimeType.JSON);
}

function isUserExists(sheet, userId) {
  const data = sheet.getDataRange().getValues();
  return data.some(row => row[0] === userId);
}

function setStatus(sheet, userId, status) {
  const data = sheet.getDataRange().getValues();
  for (let i = 0; i < data.length; i++) {
    if (data[i][0] === userId) {
      sheet.getRange(i + 1, 3).setValue(status);
      break;
    }
  }
}

function formatSheet() {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName(SHEET_NAME);
  const lastRow = Math.max(sheet.getLastRow(), 2);

  const header = sheet.getRange(1, 1, 1, 3);
  header.setFontWeight('bold');
  header.setBackground('#4A90D9');
  header.setFontColor('#FFFFFF');
  header.setHorizontalAlignment('center');

  sheet.setFrozenRows(1);
  sheet.autoResizeColumns(1, 3);

  const values = sheet.getRange(2, 3, lastRow - 1, 1).getValues();
  values.forEach((row, i) => {
    const cell = sheet.getRange(i + 2, 3);
    if (row[0] === 'active') {
      cell.setBackground('#C6EFCE');
      cell.setFontColor('#276221');
    } else if (row[0] === 'inactive') {
      cell.setBackground('#FFCCCC');
      cell.setFontColor('#9C0006');
    }
  });
}

function okResponse() {
  return ContentService.createTextOutput(JSON.stringify({ status: 'ok' }))
    .setMimeType(ContentService.MimeType.JSON);
}
```

---

## การ Deploy

1. เปิด Google Sheets → **Extensions → Apps Script**
2. วางโค้ดด้านบน
3. **Deploy → New deployment**
   - Type: **Web app**
   - Execute as: **Me**
   - Who has access: **Anyone**
4. กด **Deploy** → Authorize → Copy URL

## การอัปเดตโค้ด

1. แก้โค้ดใน Apps Script
2. **Deploy → Manage deployments → Edit (✏️)**
3. Version: **New version**
4. กด **Deploy**

---

## หมายเหตุ

- LINE webhook verification จะขึ้น 302 Found — ปกติครับ ไม่กระทบการทำงาน
- Webhook events จริง (follow, message, unfollow) ทำงานได้ตามปกติ
