# GitHub Actions — LINE Daily Notification

ส่งข้อความ LINE อัตโนมัติทุกวันจันทร์-ศุกร์ เวลา 8:30 น. โดยไม่ต้องรัน server

---

## ขั้นตอนการตั้งค่า

### 1. Push โค้ดขึ้น GitHub

```bash
git init
git add .
git commit -m "init"
git remote add origin https://github.com/<username>/<repo>.git
git push -u origin main
```

### 2. เพิ่ม Secrets

ไปที่ **Settings → Secrets and variables → Actions → New repository secret**

| Name | Value |
|---|---|
| `LINE_CHANNEL_ACCESS_TOKEN` | Channel access token จาก LINE Developers Console |
| `LINE_USER_ID` | LINE User ID ที่ต้องการส่งหา |
| `NOTIFICATION_MESSAGE` | ข้อความที่จะส่ง |

### 3. Workflow file

ไฟล์อยู่ที่ `.github/workflows/notify.yml` — ถูกสร้างไว้แล้วในโปรเจคนี้

```yaml
on:
  schedule:
    - cron: '30 1 * * 1-5'  # 8:30 AM Bangkok (UTC+7), จันทร์-ศุกร์
  workflow_dispatch:          # รันมือได้เพื่อทดสอบ
```

### 4. ทดสอบ

ไปที่แท็บ **Actions** → เลือก **LINE Daily Notification** → กด **Run workflow**

---

## เปลี่ยนเวลา

แก้ค่า cron ใน `notify.yml` โดย **ลบ 7** จากเวลา Bangkok เพื่อให้ได้ UTC

| ต้องการส่งเวลา (Bangkok) | ค่า cron (UTC) |
|---|---|
| 8:00 น. | `0 1 * * 1-5` |
| 8:30 น. | `30 1 * * 1-5` |
| 9:00 น. | `0 2 * * 1-5` |

---

## หมายเหตุ

- GitHub Actions ฟรี 2,000 นาที/เดือน (งานนี้ใช้แค่ไม่กี่วินาทีต่อครั้ง)
- เวลาอาจคลาดเคลื่อนได้ไม่กี่นาทีในช่วง GitHub มี load สูง
- ไม่ต้องรัน NestJS server ทิ้งไว้
