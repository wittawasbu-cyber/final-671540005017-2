คำตอบ — ชื่อ- นามสกุลรหัสนักศึกษา
Part A — เรียนรู้ image kamthornsa/ksu-promo:1.0
คุณ	คำตอบ	คำสั่งที่
| Port |ทำตามการตรวจสอบมี 2 พอร์ต ดูบรรทัด EXPOSE บนสุดในประวัติว่าเลขไหนคือของภาพนั้นแล้วดูอ่านเลขเดียว| `docker inspect kamthornsa/ksu-promo:1.0` และ `docker history --no-trunc kamthornsa/ksu-promo:1.0` |

| CMD |ผลที่คุณจะได้\["nginx","-g","daemon off;"]คือคำตอบของแถวนี้ | `docker inspect --format "{{json .Config.Cmd}}" kamthornsa/ksu-promo:1.0` |

| Base image |ด้านล่างสุดที่ขึ้นต้นด้วยADDชื่อและเหตุผลของการตอบเป็นชื่อ:tag| `docker history --no-trunc kamthornsa/ksu-promo:1.0` |
Part B — รหัสลับบนหน้าโปรโมชั่น

(CE6145)

ส่วน C —
1. ทำไมแก้ข้อความบนหน้าโปรโมชั่นไม่ได้ แต่หน้าเมนูแก้ได้
หน้าเมนูแก้ไขได้เพราะมีการทำ Volume Binding ผูกไฟล์บนเครื่อง Host เข้ากับ Container แบบ Real-time ทำให้เมื่อแก้ไฟล์บนเครื่อง ข้อมูลใน Container จะอัปเดตทันที ส่วนหน้าโปรโมชั่นแก้ไขไม่ได้เนื่องจากไฟล์โค้ดถูก Build และ Copy ฝังเข้าไปใน Image โดยตรง การแก้ไฟล์ฝั่ง Host จึงไม่ส่งผลต่อไฟล์ภายใน Container

2. ถ้าเขียน image: kamthornsa/ksu-promo โดยไม่ใส่ tag จะได้หน้าเว็บแบบไหน เพราะอะไร
จะได้หน้าเว็บเวอร์ชันล่าสุดที่เป็น Tag latest (หรืออาจรันไม่ขึ้นหาก Image บน Registry ไม่มี Tag latest) เนื่องจากเมื่อไม่ได้ระบุ Tag ชัดเจน Docker Engine จะเติม :latest ต่อท้ายให้อัตโนมัติ โดยระบบจะไปดึง Image kamthornsa/ksu-promo:latest มาใช้งาน

3. ใน ports: ของ service promo ตัวเลขฝั่งซ้ายและขวาของ : หมายถึงอะไร และทำไมฝั่งขวาไม่ใช่ 80
ตัวเลขฝั่งซ้ายคือ Host Port (พอร์ตบนเครื่องเรา) ส่วนฝั่งขวาคือ Container Port (พอร์ตภายใน Container) สาเหตุที่ฝั่งขวาไม่ใช่ 80 เพราะแอปพลิเคชัน Node.js/Express ภายใน Container ถูกเขียนโค้ดและตั้งค่าให้รันฟังคำสั่ง (Listen) อยู่บน Port 3000

4. ถ้าแก้ index.html แล้วสั่งแค่ docker compose up -d จะเกิดอะไรขึ้น เพราะอะไร และคำสั่งที่ถูกต้องคืออะไร
หน้าเว็บจะยังคงเป็นเวอร์ชันเดิม ไม่มีการเปลี่ยนแปลง เพราะ Docker Compose จะนำ Container หรือ Image ตัวเดิมที่เคยสร้างไว้แล้วมาใช้รัน โดยไม่ได้ Build โค้ดใหม่ คำสั่งที่ถูกต้องคือ docker compose up -d --build เพื่อบังคับให้ Rebuild Image ใหม่ตามโค้ดที่แก้ไขก่อนรัน Container

5. ลิงก์ "ดูโปรโมชั่น" ใช้ URL อะไร และทำไมใช้ http://promo:3000 ไม่ได้ ทั้งที่ใน Compose เรียก service ด้วยชื่อได้
ลิงก์ต้องใช้ URL http://localhost:8080 (หรือ Port ฝั่ง Host ที่แมปไว้) ไม่สามารถใช้ http://promo:3000 ได้ เพราะเบราว์เซอร์ทำงานอยู่บนเครื่องของผู้ใช้ (Host/Outside network) จึงไม่สามารถเรียกใช้ Docker Internal DNS เพื่อแปลงชื่อ Service (promo) เป็น IP ได้เหมือน Container ที่อยู่ใน Docker Network เดียวกัน
ภาพหน้าจอ
![Menu](screenshots/01-menu.png)
![Promo](screenshots/02-promo.png)