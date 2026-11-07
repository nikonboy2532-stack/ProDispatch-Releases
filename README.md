# 📦 ProDispatch - Official Releases & PC Host Installer
**ระบบบริหารจัดการและจ่ายงานช่างภาคสนาม & คลังข้อมูลประจำออฟฟิศ**  
*Official Release Distribution by nikonboy2532-stack*

---

## 🚀 ดาวน์โหลดตัวติดตั้งสำหรับคอมพิวเตอร์ออฟฟิศ (PC Host)

| รุ่น / เวอร์ชัน | ระบบปฏิบัติการ | ไฟล์ดาวน์โหลด | ขนาด | สถานะ |
| :--- | :--- | :--- | :--- | :--- |
| **v1.0.0 (เสถียร)** | Windows 10 / 11 (64-bit) | [📥 ดาวน์โหลด ProDispatch-Office-Host-Setup-v1.0.0.exe](https://github.com/nikonboy2532-stack/ProDispatch-Releases/releases/download/v1.0.0/ProDispatch-Office-Host-Setup-v1.0.0.exe) | 38.5 MB | ✅ แนะนำใช้งาน |

> **SHA-256 Checksum**: `2B38EF035C139BC6C46EE8138AE427F505710597A599147AD23BCC0105E4FE59`

---

## 🌟 จุดเด่นของตัวติดตั้ง PC Host Installer (`PC.exe`)

1. **ติดตั้งง่ายแบบ 1-Click**: รวมทุกโปรแกรมที่จำเป็น (Portable Node.js LTS, Express Server, Cloudflare Tunnel, Web UI) ไว้ในไฟล์ `.exe` เดียว ไม่ต้องติดตั้งโปรแกรมเสริมใดๆ ก่อน
2. **คลังข้อมูลอัตโนมัติบนเครื่องออฟฟิศ**:
   - จัดเก็บข้อมูลทั้งหมดไว้ที่ `D:\ProDispatch_Office_Data` (หรือ `C:\`)
   - แยกโฟลเดอร์ตามรหัสงานช่าง (`Jobs/<jobCode>/`) เช่น รูปถ่าย Before/After, แปลนงาน, ใบเสนอราคา, ใบสั่งซื้อ (PO) และสลิปโอนเงิน
   - สำรองข้อมูลงานทั้งหมดเป็นไฟล์ JSON ประจำวัน (`Backups/`)
   - ส่งออกรายงานสรุปงานสำหรับฝ่ายบัญชีและผู้บริหารเป็น Excel/CSV (`Reports/`)
3. **ระบบ Auto-Start & Never Sleep**:
   - เปิดระบบคลังข้อมูลทำงานเองอัตโนมัติในเบื้องหลังทุกครั้งที่เปิดเครื่องคอมพิวเตอร์
   - ตั้งค่า Power Plan ป้องกันไม่ให้คอมพิวเตอร์เข้าสู่โหมด Sleep ระหว่างเปิดเครื่องทิ้งไว้
4. **เชื่อมต่อช่างหน้างานผ่าน HTTPS ปลอดภัย (Cloudflare Tunnel)**:
   - ช่างหน้างานสามารถถ่ายรูปและส่งไฟล์เข้าเครื่องคอมที่ออฟฟิศได้จากทุกที่ทั่วประเทศ แม้ออฟฟิศจะไม่มี Fixed IP หรือไม่ได้ Forward Port
5. **หน้าจอศูนย์ควบคุมบนเครื่องแม่ (Control Dashboard)**:
   - เปิดดูสถานะการทำงานผ่านเบราว์เซอร์ได้ที่ `http://localhost:5000`
   - มีปุ่มสั่งเปิดโฟลเดอร์ข้อมูลในคอมพิวเตอร์ทันที, สั่งซิงค์ข้อมูลสด, และตรวจเช็คอัปเดตเวอร์ชันใหม่

---

## 📘 คู่มือการตั้งค่าระบบใหม่ (Out-of-the-Box Setup)

### 1. การเข้าสู่ระบบครั้งแรก
- **URL ระบบเว็บ**: `https://pd-provide-dispatch.web.app` (หรือผ่าน LAN `http://localhost:5000/app`)
- **บัญชีผู้ดูแลระบบ (Admin)**:
  - ชื่อผู้ใช้: `admin`
  - รหัสผ่าน: `123456`
  - PIN: `123456`

### 2. การผูกบัญชี Firebase Realtime Database
1. เข้าไปที่ [Firebase Console](https://console.firebase.google.com/) แล้วสร้างโปรเจกต์ใหม่
2. เปิดใช้งาน **Realtime Database** (เลือก Server โซน Singapore)
3. กำหนด Security Rules:
   ```json
   {
     "rules": {
       ".read": true,
       ".write": true
     }
   }
   ```
4. คัดลอกค่า Config จาก Project Settings ไปกรอกในไฟล์ `.env` ของระบบ

### 3. การผูกระบบแจ้งเตือน LINE ด้วย Google Apps Script
1. สร้าง **LINE Official Account** และขอ Messaging API Channel Access Token
2. สร้าง Google Apps Script Web App และวางโค้ด Webhook Relay จากโปรเจกต์
3. นำ Web App URL (ลงท้ายด้วย `/exec`) มาระบุในเมนู **จัดการความปลอดภัย (Admin) -> LINE Webhook Settings**

---

## 🔄 การอัปเดตเวอร์ชันอัตโนมัติ (Update Bridge)

ตัวโปรแกรมและหน้าเว็บ ProDispatch เชื่อมต่อกับไฟล์ `update-manifest.json` ในคลังนี้ เมื่อมีการออกอัปเดตรุ่นใหม่ (เช่น v1.0.1):
- หน้าเว็บจะแจ้งเตือนป้ายอัปเดต
- หน้าศูนย์ควบคุมเครื่องแม่จะมีปุ่มแจ้งเตือนให้กดดาวน์โหลดตัวติดตั้งใหม่มาลงทับได้ทันที โดย **ข้อมูลงานเก่าและไฟล์ทั้งหมดในฮาร์ดดิสก์จะไม่สูญหาย**

---
*สงวนลิขสิทธิ์ © 2026 ProDispatch Distribution Platform*
