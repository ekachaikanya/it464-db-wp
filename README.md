# it464-db-wp
Week06: Docker Compose : สร้าง DB Server และสร้าง Wordpress

# 📂 โปรเจกต์ WordPress + MySQL + phpMyAdmin (Docker Compose)

โปรเจกต์นี้เป็นการตั้งค่าระบบเว็บไซต์ **WordPress** แบบแยกส่วน (Microservices) โดยใช้ Docker Compose เพื่อจัดการคอนเทนเนอร์ ฐานข้อมูล และระบบเครือข่ายให้ทำงานร่วมกันอย่างปลอดภัย

โปรเจกต์สำหรับรัน **WordPress** พร้อม **MySQL** และ **phpMyAdmin** โดยเน้นความปลอดภัยด้วยการใช้ **Docker Secrets** เพื่อซ่อนรหัสผ่าน และ **Docker Configs** สำหรับตั้งค่า PHP

---

## 🛠️ โครงสร้างบริการ (Services)


| Service | Container Name | Port | หน้าที่ |
| :--- | :--- | :--- | :--- |
| **WordPress** | `mywp` | `8000` | ตัวเว็บไซต์หลัก (รองรับไฟล์อัปโหลดขนาดใหญ่) |
| **MySQL** | `mydb` | `3306` | ฐานข้อมูล (รองรับการอ่านรหัสจากไฟล์ลับ) |
| **phpMyAdmin** | `mypma` | `8081` | เครื่องมือจัดการ DB ผ่านเว็บ |

---

## 🔒 ระบบความปลอดภัยและตั้งค่า (Secrets & Configs)

โปรเจกต์นี้ไม่ได้พิมพ์รหัสผ่านลงในไฟล์ YAML โดยตรง แต่ใช้ระบบไฟล์ลับของ Docker:

1.  **Secrets:** รหัสผ่านจะถูกอ่านจากไฟล์ `.secret` และเก็บไว้ใน Memory (`/run/secrets/`) เพื่อป้องกันการรั่วไหล
2.  **Configs:** แยกไฟล์ `uploads.ini` ออกมาเพื่อให้ปรับแต่งค่า PHP (เช่น `upload_max_filesize`) ได้ง่ายโดยไม่ต้องแก้ Image

---

## 📂 การเตรียมไฟล์ก่อนเริ่มรัน

เพื่อให้ระบบทำงานได้ คุณต้องมีไฟล์ทั้งหมดในโฟลเดอร์เดียวกันดังนี้:

1.  **`compose.yaml`**: ไฟล์หลักสำหรับคุมระบบ
2.  **`.db_root.secret`**: ไฟล์เก็บรหัสผ่าน Root ของ MySQL (เช่น `somewordpress`)
3.  **`.db_pass.secret`**: ไฟล์เก็บรหัสผ่านสำหรับ User ทั่วไป (เช่น `wp1234`)
4.  **`uploads.ini`**: ไฟล์ตั้งค่า PHP (เช่น `upload_max_filesize = 64M`)

---

## 🚀 วิธีการใช้งาน

1.  **สั่งรันระบบ:**
    ```bash
    docker compose up -d
    ```
2.  **ตรวจสอบสถานะ:**
    ```bash
    docker compose ps
    ```
3.  **การเข้าใช้งาน:**
    *   🌐 **WordPress:** [http://localhost:8000](http://localhost:8000)
    *   🗄️ **phpMyAdmin:** [http://localhost:8081](http://localhost:8081)

---

## 🌐 เครือข่ายและการจัดเก็บข้อมูล (Networks & Volumes)

*   **Networks:** แยกวง `frontend-net` (หน้าบ้าน) และ `backend-net` (หลังบ้าน) เพื่อจำกัดสิทธิ์การเข้าถึงฐานข้อมูล
*   **Volumes:** 
    *   `db_data`: เก็บข้อมูล MySQL ถาวร
    *   `wp_data`: เก็บไฟล์ WordPress (Themes/Plugins/Uploads)

---

## ⚠️ ข้อควรระวัง (Best Practices)

*   **ห้ามลบไฟล์ .secret:** หากลบไฟล์เหล่านี้ ระบบจะไม่สามารถสตาร์ทคอนเทนเนอร์ขึ้นมาได้
*   **Git Security:** ตรวจสอบให้แน่ใจว่าได้เพิ่มไฟล์ `*.secret` ลงใน [`.gitignore`](https://docs.github.com) เพื่อไม่ให้รหัสผ่านหลุดขึ้นอินเทอร์เน็ต

---
*จัดทำตามมาตรฐาน [Docker Compose Specification](https://docs.docker.com)*
---
*เอกสารนี้จัดทำเพื่ออธิบายโครงสร้างระบบตามมาตรฐาน [Docker Documentation](https://docs.docker.com)*
