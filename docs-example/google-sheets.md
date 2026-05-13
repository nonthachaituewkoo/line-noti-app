# Google Sheets Setup

เก็บรายชื่อ userId ของผู้ใช้ที่ subscribe รับ LINE notification

---

## Sheet ID

ดูได้จาก URL ของ Google Sheets:

```
https://docs.google.com/spreadsheets/d/YOUR_SHEET_ID/edit
```

## โครงสร้างตาราง

Sheet name: `userId`

| คอลัมน์ | ชื่อ | คำอธิบาย |
| --- | --- | --- |
| A | userId | LINE User ID ของผู้ใช้ |
| B | registeredAt | วันเวลาที่ add bot |
| C | status | `active` หรือ `inactive` |

## ตัวอย่าง

| userId | registeredAt | status |
| --- | --- | --- |
| Uxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx | 13/5/2026, 14:37:57 | active |
| Uyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy | 13/5/2026, 15:00:00 | inactive |

## การจัดการ status

| เหตุการณ์ | status |
| --- | --- |
| ผู้ใช้ add bot / ส่งข้อความ | `active` |
| ผู้ใช้ block / unfollow bot | `inactive` |

GitHub Actions จะส่งข้อความเฉพาะแถวที่ status เป็น `active` เท่านั้น

---

## การ Share ให้ Service Account

Share Sheet ให้ email ของ Service Account เป็น **Editor**:

```
your-service-account@your-project.iam.gserviceaccount.com
```
