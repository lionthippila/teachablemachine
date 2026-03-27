# 🤖 Teachable Machine Pose Control to ESP32 via MQTT

โปรเจกต์เว็บแอปพลิเคชันสำหรับตรวจจับท่าทาง (Pose Detection) ด้วยโมเดลจาก Google Teachable Machine และส่งคำสั่งควบคุม (เช่น Forward, Backward, Stop, Left, Right) ไปยังบอร์ดไมโครคอนโทรลเลอร์ ESP32 ผ่านโปรโตคอล MQTT

## ✨ ฟีเจอร์หลัก (Features)
* **Pose Detection:** ใช้ TensorFlow.js และโมเดลจาก Teachable Machine ในการตรวจจับร่างกาย
* **Minimalist UI:** อินเทอร์เฟซเรียบง่าย สะอาดตา พร้อมแสดงสถานะการเชื่อมต่อ MQTT
* **Smart Trigger (หน่วงเวลา):** ป้องกันการส่งคำสั่งรัวเกินไป (Spam) โดยระบบจะส่งคำสั่งผ่าน MQTT ก็ต่อเมื่อผู้ใช้ **ค้างท่าทางนั้นไว้ครบ 2 วินาที** และความแม่นยำต้องมากกว่า 90%
* **Real Left/Right:** ตั้งค่ากล้องไม่ให้เป็นกระจกสะท้อน (`flip = false`) เพื่อให้การทำท่าซ้าย-ขวา ตรงกับความเป็นจริง
* **WebSockets MQTT:** เชื่อมต่อกับ MQTT Broker ฟรีผ่าน WebSockets โดยตรงจากหน้าเว็บเบราว์เซอร์

## 🎮 แบบจำลองวงจร (Simulation)
สามารถดูการต่อวงจรและทดลองรันโค้ดฝั่ง ESP32 ได้ที่ Wokwi:
👉 **[คลิกเพื่อดูโปรเจกต์บน Wokwi](https://wokwi.com/projects/459661529875493889)**

## 🛠️ การตั้งค่าระบบ (Configuration)
หากต้องการนำไปใช้งานหรือปรับแต่ง คุณสามารถแก้ไขค่าในไฟล์ `index.html` ได้ดังนี้:

* **URL ของโมเดล:** เปลี่ยนลิงก์โมเดลของคุณในตัวแปร `const URL`
* **MQTT Broker:** ปัจจุบันใช้เซิร์ฟเวอร์ฟรีคือ `wss://broker.emqx.io:8084/mqtt`
* **MQTT Topic:** ปัจจุบันตั้งค่าส่งข้อมูลไปที่หัวข้อ `test/control` (สามารถแก้ได้ที่ตัวแปร `const mqttTopic`)
* **เวลาหน่วง (Hold Time):** ปัจจุบันตั้งไว้ที่ 2 วินาที แก้ไขได้ที่ตัวแปร `const HOLD_TIME_MS = 2000;`

## 🚀 วิธีใช้งาน (How to use)
1. ดาวน์โหลดหรือ Clone โปรเจกต์นี้ลงเครื่อง
2. เปิดไฟล์ `index.html` ด้วยเว็บเบราว์เซอร์ (แนะนำ Google Chrome)
3. กดยอมรับการเข้าถึงกล้อง (Allow Camera)
4. กดปุ่ม **Start Camera** เพื่อเริ่มการทำงาน
5. รอจนกว่าข้อความสถานะจะเปลี่ยนเป็น **MQTT Connected ✅**
6. ทำท่าทางและค้างไว้ 2 วินาที ระบบจะส่งคำสั่ง (พิมพ์ใหญ่ทั้งหมด เช่น `FORWARD`) ไปยัง ESP32 

## 🔌 สำหรับฝั่ง ESP32 (Hardware Side)
บอร์ด ESP32 ของคุณจะต้องเขียนโค้ดเพื่อเชื่อมต่อ WiFi และเชื่อมต่อ MQTT ไปที่:
* **Broker:** `broker.emqx.io`
* **Port:** `1883` (สำหรับ ESP32 ปกติจะใช้ TCP Port 1883)
* **Subscribe Topic:** `test/control`
เมื่อ ESP32 ได้รับข้อความ เช่น `FORWARD` หรือ `LEFT` ให้นำข้อความนั้นไปสร้างเงื่อนไข `if-else` เพื่อสั่งงาน Motor Driver ต่อไป

---
*Developed with JavaScript, TensorFlow.js, and MQTT.js*