# GitHub Actions Setup

รัน workflow อัตโนมัติทุกวันจันทร์-ศุกร์ เวลา 8:00 น. เพื่อส่ง LINE notification

---

## Secrets ที่ต้องตั้ง

Settings → Secrets and variables → Actions → New repository secret

| Name | Value |
| --- | --- |
| `LINE_CHANNEL_ACCESS_TOKEN` | Channel access token จาก LINE Developers Console |
| `NOTIFICATION_MESSAGE` | ข้อความที่จะส่งทุกวัน |

---

## Workflow (notify.yaml)

```yaml
on:
  schedule:
    - cron: '0 1 * * 1-5'  # 8:00 AM Bangkok (UTC+7), Mon-Fri
  workflow_dispatch:        # รันมือได้ผ่านแท็บ Actions
```

### ขั้นตอนการทำงาน

1. **Get active user IDs** — เรียก Apps Script URL เพื่อดึงรายชื่อ userId ที่ status เป็น `active`
2. **Send LINE Notifications** — ส่งข้อความหาทุก userId ที่ได้มา

---

## การเปลี่ยนเวลา

แก้ค่า cron ใน `.github/workflows/notify.yaml` (เวลา UTC = Bangkok - 7)

| เวลา Bangkok | cron (UTC) |
| --- | --- |
| 8:00 น. | `0 1 * * 1-5` |
| 8:30 น. | `30 1 * * 1-5` |
| 9:00 น. | `0 2 * * 1-5` |

## การเปลี่ยนข้อความ

แก้ค่า `NOTIFICATION_MESSAGE` ใน GitHub Secrets ได้เลย ไม่ต้อง push โค้ด

---

## ข้อจำกัด

- เวลาอาจคลาดเคลื่อน 5-15 นาที ช่วง GitHub มี load สูง
- Free tier 2,000 นาที/เดือน (งานนี้ใช้ไม่กี่วินาทีต่อครั้ง)
- **Repo ที่ไม่มี activity นาน 60 วัน — GitHub จะ disable workflow อัตโนมัติ**
