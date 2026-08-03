---
name: scrutinize
description: ตรวจสอบ plan, PR, diff, design document หรือ code change แบบ end-to-end จากมุมมองคนนอก โดยทบทวน intent มองหาวิธีที่เล็กกว่า ไล่ actual code path และยืนยันว่า change ทำตามที่กล่าวอ้างจริง ใช้เมื่อผู้ใช้ขอ review, audit, sanity-check, second opinion, ตรวจแผน หรือตรวจความเสี่ยงก่อน merge โดย default ให้รายงานอย่างเดียวและไม่แก้ไฟล์
---

# Scrutinize

ตรวจ change โดยแยกสิ่งที่ผู้เขียนกล่าวอ้างออกจากสิ่งที่พิสูจน์ได้จริง มองให้พ้นบริเวณ diff แต่รักษาขอบเขตตามเป้าหมายของผู้ใช้

## 1. ยืนยัน intent และขอบเขต

1. สรุปเป้าหมายของ change เป็นหนึ่งประโยคด้วยถ้อยคำของตนเอง
2. ระบุ artifact ที่ต้องตรวจ เช่น plan, branch, PR, staged diff หรือไฟล์ที่เจาะจง
3. ถ้าอธิบายเป้าหมายไม่ได้จากข้อมูลที่มี ให้บอกข้อมูลที่ขาดและถามคำถามที่เฉพาะเจาะจงก่อน review ต่อ
4. ถือว่าเป็นงาน read-only เว้นแต่ผู้ใช้สั่งให้แก้ไขอย่างชัดเจน

## 2. ท้าทายแนวทางก่อนตรวจรายละเอียด

ตรวจว่าปัญหานั้นมีอยู่จริงและ change นี้จำเป็นหรือไม่ จากนั้นพิจารณาทางเลือกต่อไปนี้:

- ใช้ capability ที่มีอยู่แล้วแทนการเพิ่ม surface ใหม่
- ลด change ให้เป็น slice ที่เล็กกว่าและตรวจสอบได้ง่ายกว่า
- ย้ายวิธีแก้ไปยัง layer ที่เหมาะกว่า เช่น config, framework, build หรือ runtime
- ไม่ทำ change เมื่อผลประโยชน์ไม่คุ้มกับ complexity และ regression risk

ถ้ามีทางเลือกที่ดีกว่า ให้รายงานก่อน findings ระดับบรรทัด พร้อมอธิบาย trade-off ที่เป็นรูปธรรม อย่าเสนอทางเลือกเพียงเพื่อให้ดูว่ามีตัวเลือก

## 3. ไล่ actual code path

สำหรับ behavior สำคัญแต่ละข้อ ให้ตามเส้นทางจริงตั้งแต่ต้นจนจบ:

1. entry point และ caller ที่เกี่ยวข้อง
2. branch, guard, config และ state ที่กำหนดเส้นทาง
3. state mutation, persistence, network call หรือ side effect
4. error path, retry, cleanup และค่าที่ส่งกลับ
5. unchanged code ก่อนและหลังจุดที่แก้ เพราะ defect มักอยู่ตรงรอยต่อ

สำหรับ plan หรือ design ให้เทียบ proposed flow กับระบบปัจจุบัน และระบุ assumption ที่ยังไม่มีหลักฐานรองรับ

## 4. ตรวจ claims และความเสี่ยง

แยกแต่ละ claim ออกจากผลการตรวจ แล้วตอบให้ได้ว่า:

- code path สร้าง behavior ตาม claim จริงหรือไม่
- input หรือ state ใดทำให้พัง เช่น empty, null, unicode, ข้อมูลขนาดใหญ่, concurrency, partial failure หรือ out-of-order event
- change กระทบ contract, error semantics, performance, observability, storage หรือ wire format โดยไม่ตั้งใจหรือไม่
- tests วิ่งผ่าน path ที่ต้องการจริง หรือ mock/fixture ทำให้ข้ามจุดเสี่ยง
- สิ่งใดยังตรวจไม่ได้ และต้องใช้หลักฐานอะไรเพิ่ม

อย่าเปลี่ยนความไม่แน่ใจให้เป็นข้อเท็จจริง ให้ระบุระดับหลักฐานและข้อจำกัดของ review อย่างตรงไปตรงมา

## 5. รายงานผล

เรียง findings ตามระดับ `blocker`, `major`, `minor`, `nit` และใช้หนึ่งหัวข้อต่อหนึ่งปัญหา แต่ละ finding ต้องมี:

- **Finding** — ปัญหาที่เฉพาะเจาะจง พร้อม `file:line` หรือ path เมื่อมี
- **Impact** — ผลที่เกิดกับผู้ใช้ ระบบ หรือการดูแลรักษา
- **Evidence** — code path, state หรือ test case ที่แสดงปัญหา
- **Suggested change** — วิธีแก้ที่เล็กและตรวจสอบได้

ปิดท้ายด้วย verdict หนึ่งบรรทัด: `ship`, `fix-then-ship`, `rework` หรือ `reject` พร้อมเหตุผลหลักหนึ่งข้อ

ถ้าไม่พบปัญหา อย่าตอบเพียง `LGTM` ให้สรุป paths, claims และ edge cases ที่ตรวจแล้ว รวมถึงสิ่งที่อยู่นอก coverage
