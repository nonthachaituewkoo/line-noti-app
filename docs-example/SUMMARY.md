# สรุป LINE Daily Notification

ส่งข้อความ LINE อัตโนมัติทุกวันจันทร์-ศุกร์ เวลา 8:00 น. โดยไม่ต้องรัน server

---

## ภาพรวมระบบ

```text
ผู้ใช้ add bot → ส่งข้อความมา
→ LINE ส่ง webhook → Apps Script รับ
→ บันทึก userId ลง Google Sheets

Apps Script Time Trigger (ทุกวัน 8:00 น. จ-ศ)
→ ดึง userId ที่ status = active จาก Sheets
→ ส่ง LINE ทีละคน
```

---

## เอกสารแยกหัวข้อ

| หัวข้อ | ไฟล์ | คำอธิบาย |
| --- | --- | --- |
| LINE Bot & Webhook | [line-webhook.md](line-webhook.md) | ตั้งค่า webhook URL และ LINE channel |
| Apps Script (Webhook) | [apps-script.md](apps-script.md) | โค้ด, deployment, Web App URL |
| Apps Script (Notification) | [app-script-line-notify.md](app-script-line-notify.md) | ส่ง LINE ผ่าน Time Trigger |
| Google Sheets | [google-sheets.md](google-sheets.md) | โครงสร้างตาราง |
| GitHub Actions | [github-actions.md](github-actions.md) | Workflow อ้างอิง (ไม่ได้ใช้งานแล้ว) |

---

## ข้อจำกัดสำคัญ

- LINE free tier **500 messages/เดือน** (ส่ง จ-ศ ใช้ ~22 ครั้ง/เดือน)
- Apps Script Time Trigger delay ไม่เกิน 1-2 นาที
- Apps Script ฟรี แต่มี quota การเรียก UrlFetchApp 20,000 ครั้ง/วัน
