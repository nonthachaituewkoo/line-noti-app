# Apps Script — LINE Notification

ส่งข้อความ LINE อัตโนมัติทุกวันจันทร์-ศุกร์ เวลา 8:00 น. ผ่าน Apps Script Time Trigger
แม่นยำกว่า GitHub Actions scheduler ที่อาจ delay ได้หลายชั่วโมง

---

## โค้ด

เพิ่มใน `รหัส.gs` ต่อจากโค้ดเดิม:

```javascript
function sendNotifications() {
  const props = PropertiesService.getScriptProperties();
  const token = props.getProperty('LINE_CHANNEL_ACCESS_TOKEN');
  const message = props.getProperty('NOTIFICATION_MESSAGE');

  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName(SHEET_NAME);
  const data = sheet.getDataRange().getValues();
  const activeUsers = data.slice(1).filter(row => row[2] === 'active').map(row => row[0]);

  const day = new Date().getDay();
  if (day === 0 || day === 6) return; // หยุดเสาร์-อาทิตย์

  activeUsers.forEach(userId => {
    UrlFetchApp.fetch('https://api.line.me/v2/bot/message/push', {
      method: 'post',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${token}`
      },
      payload: JSON.stringify({
        to: userId,
        messages: [{ type: 'text', text: message }]
      })
    });
  });
}
```

---

## Script Properties

Apps Script → **Project Settings (ฟันเฟือง)** → **Script Properties** → Add script property

| Property | Value |
| --- | --- |
| `LINE_CHANNEL_ACCESS_TOKEN` | Channel access token จาก LINE Developers Console |
| `NOTIFICATION_MESSAGE` | ข้อความที่จะส่งทุกวัน |

---

## ตั้ง Time Trigger

Apps Script → **Triggers (ไอคอนนาฬิกา)** → **Add Trigger**

| ค่า | เลือก |
| --- | --- |
| Choose function | `sendNotifications` |
| Select event source | **Time-driven** |
| Select type | **Day timer** |
| Select time | **8am to 9am** |

กด **Save** → Authorize

---

## การเปลี่ยนข้อความ

แก้ค่า `NOTIFICATION_MESSAGE` ใน **Script Properties** ได้เลย ไม่ต้อง deploy ใหม่

---

## หมายเหตุ

- Apps Script Time Trigger แม่นยำกว่า GitHub Actions มาก (delay ไม่เกิน 1-2 นาที)
- Trigger รัน Day timer ทุกวัน แต่โค้ดจะ return ทันทีถ้าเป็นเสาร์-อาทิตย์
- ทดสอบได้โดยเลือก function `sendNotifications` แล้วกด **Run** ใน Apps Script
