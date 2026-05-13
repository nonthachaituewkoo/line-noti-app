# สรุป LINE Daily Notification

ส่งข้อความ LINE อัตโนมัติทุกวันจันทร์-ศุกร์ เวลา 8:00 น. โดยไม่ต้องรัน server

---

## ภาพรวมระบบ

```text
ผู้ใช้ add bot → ส่งข้อความมา
→ LINE ส่ง webhook → Apps Script รับ
→ บันทึก userId ลง Google Sheets

GitHub Actions (ทุกวัน 8:00 น. จ-ศ)
→ ดึง userId ที่ status = active จาก Apps Script
→ ส่ง LINE ทีละคน
```

---

## เอกสารแยกหัวข้อ

| หัวข้อ | ไฟล์ | คำอธิบาย |
| --- | --- | --- |
| LINE Bot & Webhook | [line-webhook.md](line-webhook.md) | ตั้งค่า webhook URL และ LINE channel |
| Apps Script | [apps-script.md](apps-script.md) | โค้ด, deployment, Web App URL |
| Google Sheets | [google-sheets.md](google-sheets.md) | โครงสร้างตาราง, Sheet ID |
| GitHub Actions | [github-actions.md](github-actions.md) | Workflow, Secrets, การเปลี่ยนเวลา/ข้อความ |

---

## ข้อจำกัดสำคัญ

- Repo ไม่มี activity **60 วัน** → GitHub disable workflow อัตโนมัติ ต้องเปิดใหม่ที่แท็บ Actions
- LINE free tier **500 messages/เดือน** (ส่ง จ-ศ ใช้ ~22 ครั้ง/เดือน)
- เวลาส่งอาจคลาดเคลื่อน 5-15 นาที ช่วง GitHub load สูง
