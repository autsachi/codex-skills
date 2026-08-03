---
name: management-talk
description: แปลงข้อมูล engineer-to-engineer เป็นอัปเดตที่ชัดเจนสำหรับ engineering leadership, PM, director, release manager หรือผู้บริหาร และปรับรูปแบบให้เหมาะกับ JIRA, Slack, async standup, email หรือ meeting talking points ใช้เมื่อผู้ใช้ขอ executive summary, leadership update, status update, สรุปให้ผู้บริหาร, ลดศัพท์เทคนิค หรือเรียบเรียงข้อความวิศวกรรมสำหรับช่องทางสื่อสาร
---

# Management Talk

สื่อสาร engineering truth ให้ผู้บริหารตัดสินใจได้ โดยรักษาความถูกต้องของสถานะ ผลกระทบ owner ความเสี่ยง และ next steps แต่ตัดรายละเอียดระดับ implementation ที่ไม่ช่วยการตัดสินใจ

## ระบุผู้รับและช่องทาง

หาคำตอบให้ได้ว่าเนื้อหาจะส่งให้ใครและผ่านช่องทางใด หากยังไม่รู้ช่องทาง ให้ถามคำถามเดียวที่สั้นและเฉพาะเจาะจงก่อนเขียน เช่น “ต้องการฉบับ JIRA, Slack, standup, email หรือ talking points?”

ใช้ skill นี้กับ engineering-savvy leadership เช่น PM, VP, director และ release manager ไม่ใช้เป็น default สำหรับ marketing, finance, customer announcement หรือคำอธิบายแบบ ELI5 เพราะแต่ละกลุ่มต้องการกรอบสื่อสารต่างกัน

## แปลงรายละเอียดโดยไม่บิดข้อเท็จจริง

### คงไว้

- product, framework, team และ component names ที่ผู้รับใช้ติดตามงาน
- ticket, PR และ customer/workload identifiers
- สถานะ ผลกระทบ owner next steps workaround และความเสี่ยงที่มีหลักฐาน
- คำเทคนิคระดับแนวคิดที่จำเป็น เช่น race condition, regression, synchronization หรือ rollback

### ตัดออก

- function names, local file paths, line numbers, struct fields และ commit SHA ที่ไม่จำเป็นต่อการติดตาม
- debug transcript, tool commands และขั้นตอนทดลองรายครั้ง
- รายละเอียด process ที่ไม่เปลี่ยนสถานะ ผลกระทบ หรือการตัดสินใจ

### แปลความ

อธิบาย mechanism เป็น cause-and-effect ภาษาธรรมดาหนึ่งหรือสองประโยค รักษาความหมายเดิมและไม่ลดทอนความรุนแรงของข้อเท็จจริง อย่าเปลี่ยน “race condition” เป็นคำกว้างจนผู้อ่านเข้าใจผิด

## จัดรูปตามช่องทาง

- **JIRA หรือ written status** — ใช้หัวข้อสั้น เช่น Status, Impact, Cause, Owner, Next steps, Workaround และ Risk เฉพาะที่เกี่ยวข้อง
- **Slack** — นำด้วยสถานะหนึ่งบรรทัด ตามด้วย impact และ action ที่ใกล้ที่สุด ใช้ bullets เท่าที่ช่วยให้สแกนเร็ว
- **Async standup** — สรุปสิ่งที่เสร็จ สิ่งที่ทำต่อ และ blocker โดยไม่เล่าย้อนทั้งเหตุการณ์
- **Email** — ใส่ subject ที่บอกสถานะ เปิดด้วยข้อสรุป แล้วให้ context และ action/decision ที่ต้องการ
- **Meeting talking points** — เขียนเป็นประโยคพูดสั้น ๆ เรียงจากสถานะ ผลกระทบ หลักฐาน ไปยัง next step

ปรับความยาวตามช่องทาง อย่าบังคับ template เดียวกับทุกปลายทาง

## ตรวจคุณภาพก่อนส่ง

- เริ่มด้วยข้อมูลที่ผู้รับจำเป็นต้องรู้ที่สุด
- ใช้ active voice และระบุ owner เมื่อข้อมูลมีอยู่
- แยกข้อเท็จจริง สิ่งที่ยังไม่รู้ และความเสี่ยงออกจากกัน
- อย่าสร้าง deadline, owner, customer impact หรือ confidence ที่ต้นฉบับไม่ได้ให้มา
- อย่าใช้ถ้อยคำชมเชย filler หรือ hedge ที่ไม่มีความหมาย
- อย่าสั่งผู้บริหารว่าควรตัดสินใจอย่างไร ให้ข้อเท็จจริงและ trade-off เว้นแต่ผู้ใช้ขอ recommendation
- อย่าส่งข้อความหรือแก้ ticket ภายนอกจนกว่าผู้ใช้จะอนุมัติข้อความและสั่งให้ส่งอย่างชัดเจน

ส่งออกเฉพาะข้อความที่พร้อมใช้ในช่องทางนั้น หากต้องตั้งสมมติฐาน ให้บอกสั้น ๆ ก่อน draft
