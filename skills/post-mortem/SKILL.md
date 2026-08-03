---
name: post-mortem
description: เขียน engineering post-mortem หรือ RCA ของ bug ที่แก้และ validate แล้ว โดยบันทึก symptom, root cause, กลไกของปัญหา, fix, validation และเหตุที่หลุดรอด ใช้เมื่อผู้ใช้ขอ post-mortem, root cause analysis, RCA, สรุปสาเหตุหลังแก้บั๊ก หรือทำเอกสารปิดงาน ห้ามใช้แทน incident report และห้ามเขียนจากสมมติฐานที่ยังไม่ยืนยัน
---

# Post-mortem

สร้างบันทึกทางวิศวกรรมที่ช่วยให้ผู้อ่านในอนาคตเข้าใจว่าอะไรพัง ทำไมจึงพัง แก้อย่างไร และรู้ได้อย่างไรว่าแก้แล้ว

## ตรวจ prerequisites ก่อนเขียน

ยืนยันข้อมูลสี่ข้อก่อนร่างเอกสาร:

- มีวิธี reproduce symptom ที่แน่นอนหรือมีอัตราเกิดสูงพอให้ตรวจซ้ำได้
- root cause ได้รับการยืนยันเป็นกลไก ไม่ใช่เพียง hypothesis
- ระบุ fix ด้วย PR, commit, branch หรือรายการ change ที่ชัดเจน
- validation แสดงว่า original failure หายไปและไม่ได้ผ่านเพราะหลบ path เดิม

ถ้าขาดข้อใด ให้แสดงรายการข้อมูลที่ขาดแล้วหยุด อย่าเติมช่องว่างด้วยรายละเอียดที่ฟังดูน่าเชื่อ

อย่าใช้ workflow นี้กับ customer-visible outage ที่ต้องมี timeline, blast radius, detection และ communication history ให้เสนอ incident report แทน หากเป็น typo หรือ change เล็กที่ PR description เพียงพอ อย่าสร้างเอกสารเกินความจำเป็น

## รวบรวมหลักฐาน

อ่าน repro, logs, tests, diff, PR หรือ commit ที่เกี่ยวข้อง แล้วแยกข้อมูลต่อไปนี้:

- สิ่งที่ผู้ใช้หรือระบบสังเกตเห็น
- cause chain ตั้งแต่ defect ถึง symptom
- หลักฐานที่ยืนยัน cause และ hypotheses ที่ถูกตัดออก
- เหตุผลที่ fix แก้ cause แทนการซ่อน symptom
- ขอบเขตของ validation ทั้งที่ทดสอบแล้วและยังไม่ได้ทดสอบ
- gap ใน test, review, workload หรือ process ที่ทำให้ปัญหาหลุดรอด

ใช้ code identifiers, file paths, function names และ commit references เมื่อช่วยให้วิศวกรตามกลับไปยังต้นเหตุได้

## โครงสร้างเอกสาร

เรียงหัวข้อตามลำดับนี้:

1. **Summary** — symptom, ผลกระทบ, fix และสถานะปัจจุบันในย่อหน้าเดียว
2. **Symptom** — error, log, test หรือ behavior ที่สังเกตจริง
3. **Root cause** — กลไกและ cause chain แบบ end-to-end
4. **Why it produced the symptom** — เชื่อม defect ภายในกับผลที่มองเห็น
5. **Fix** — สิ่งที่เปลี่ยนและเหตุผลที่แก้ root cause
6. **How it was found** — repro, เครื่องมือ, hypotheses ที่ตัดออก และ experiment ที่ยืนยัน
7. **Why it slipped through** — coverage หรือ process gap โดยไม่โทษบุคคล
8. **Validation** — tests, workloads, metrics หรือ soak run พร้อมขอบเขตจริง
9. **Follow-ups** — งานที่มี owner และ tracking reference หรือระบุว่าไม่มีงานต่อ

`Summary`, `Root cause`, `Fix` และ `Validation` เป็นหัวข้อบังคับ หัวข้ออื่นตัดออกได้เมื่อไม่มีสาระและต้องไม่ทำให้ข้อเท็จจริงขาดตอน

## กฎการเขียนและการส่งออก

- ใช้ active voice และถ้อยคำที่ตรวจสอบย้อนกลับได้
- รักษาหลัก blameless โดยอธิบาย defect และ system gap ไม่กล่าวโทษคน
- อย่าใช้คำว่า “น่าจะ” หรือ “เชื่อว่า” แทนหลักฐาน ถ้ายังไม่รู้ให้ระบุว่ายังไม่ยืนยัน
- ระบุ validation coverage ตามจริง และอย่าเหมารวมไปยัง configuration ที่ไม่ได้ทดสอบ
- สร้าง draft ในแชตเป็น default หากผู้ใช้ไม่ได้ระบุปลายทาง
- อย่า post, comment, ส่ง email หรือแก้เอกสารภายนอกโดยไม่มีคำสั่งและการยืนยันที่ชัดเจน
- เมื่อต้องการฉบับสำหรับ leadership ให้ส่งต่อเนื้อหาที่เสร็จแล้วไปยัง `$management-talk` โดยไม่ลดทอน engineering record ต้นฉบับ
