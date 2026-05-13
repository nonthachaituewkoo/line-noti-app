# LINE Daily Notification

ส่งข้อความ LINE อัตโนมัติทุกวันจันทร์-ศุกร์ เวลา 8:00 น. โดยไม่ต้องรัน server

---

## การทำงาน

```text
ผู้ใช้ add bot → ส่งข้อความมา
→ LINE ส่ง webhook → Apps Script รับ
→ บันทึก userId ลง Google Sheets

GitHub Actions (ทุกวัน 8:00 น. จ-ศ)
→ ดึง userId ที่ status = active จาก Apps Script
→ ส่ง LINE ทีละคน
```

---

## Stack

- **GitHub Actions** — scheduler ส่งข้อความตามเวลา
- **LINE Messaging API** — ส่งข้อความหาผู้ใช้
- **Google Apps Script** — รับ webhook และ serve รายชื่อผู้ใช้
- **Google Sheets** — เก็บ userId ของผู้ที่ subscribe

---

## Setup

### 1. Apps Script

สร้าง Google Sheets → Extensions → Apps Script → วางโค้ดจาก `docs-example/apps-script.md` → Deploy เป็น Web App (Anyone)

### 2. LINE Webhook

LINE Developers Console → Messaging API → Webhook URL → ใส่ Apps Script URL → เปิด Use webhook

### 3. GitHub Secrets

| Name | Value |
| --- | --- |
| `LINE_CHANNEL_ACCESS_TOKEN` | Channel access token จาก LINE Developers |
| `NOTIFICATION_MESSAGE` | ข้อความที่จะส่งทุกวัน |

### 4. ทดสอบ

Actions → LINE Daily Notification → **Run workflow**

---

## เอกสาร

ดูรายละเอียดแต่ละส่วนได้ที่ [`docs-example/`](docs-example/)

| ไฟล์ | เนื้อหา |
| --- | --- |
| [apps-script.md](docs-example/apps-script.md) | โค้ด Apps Script + deployment |
| [google-sheets.md](docs-example/google-sheets.md) | โครงสร้างตาราง |
| [line-webhook.md](docs-example/line-webhook.md) | LINE Bot & Webhook |
| [github-actions.md](docs-example/github-actions.md) | Workflow & Secrets |

---

## ข้อจำกัด

- เวลาอาจคลาดเคลื่อน 5-15 นาที ช่วง GitHub มี load สูง
- LINE free tier 500 messages/เดือน (ส่ง จ-ศ ใช้ ~22 ครั้ง/เดือน)
- Repo ไม่มี activity **60 วัน** → GitHub disable workflow อัตโนมัติ
