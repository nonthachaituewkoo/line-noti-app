# LINE Webhook & Bot Setup

ตั้งค่า LINE Messaging API ให้ส่ง webhook events มายัง Apps Script

---

## Channel Information

| ค่า | ตัวอย่าง |
| --- | --- |
| Provider | your-provider-name |
| Channel name | your-channel-name |
| Console URL | developers.line.biz/console/channel/YOUR_CHANNEL_ID/messaging-api |

---

## Webhook Settings

| ค่า | รายละเอียด |
| --- | --- |
| Webhook URL | Apps Script Web App URL |
| Use webhook | ON |

```text
YOUR_APPS_SCRIPT_WEB_APP_URL
```

> Verify จะขึ้น 302 Found — ปกติครับ ไม่กระทบการทำงาน

---

## วิธีหา LINE User ID

### ของตัวเอง

LINE Developers Console → Channel → Messaging API → **Your user ID**

### ของผู้ใช้คนอื่น

ผู้ใช้ add bot แล้วส่งข้อความ → Apps Script รับ webhook → บันทึก userId ลง Google Sheets อัตโนมัติ

---

## Events ที่รองรับ

| Event | การทำงาน |
| --- | --- |
| `follow` | บันทึก userId, ตั้ง status = active |
| `message` | บันทึก userId ถ้ายังไม่มี, ตั้ง status = active |
| `unfollow` | เปลี่ยน status = inactive |
