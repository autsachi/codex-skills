---
name: commit-message
description: เขียน commit message คุณภาพดีจาก file diff โดยใช้รูปแบบ Conventional Commits และใช้ภาษาไทยเป็นหลัก ยกเว้นคำศัพท์เฉพาะหรือคำที่ไม่ควรแปลให้ทับศัพท์ เช่น git diff, staged changes, scope, breaking change. ใช้เมื่อผู้ใช้ขอ commit message, commit title, commit body, Conventional Commit หรือต้องการให้ Codex อ่าน git diff/staged changes แล้วสรุปเป็น commit.
---

# Commit Message

## หลักการใช้ภาษา

- ใช้ภาษาไทยเป็นหลักในการอธิบาย เหตุผล ตัวเลือก และคำแนะนำ
- คงคำศัพท์เฉพาะเป็นภาษาอังกฤษหรือทับศัพท์เมื่อแปลแล้วเสียความหมาย เช่น `git diff`, `staged`, `unstaged`, `commit`, `scope`, `type`, `subject`, `body`, `footer`, `Conventional Commits`, `BREAKING CHANGE`
- โดย default ให้เขียน commit message เป็นภาษาไทย แต่ยังคง `type(scope):` เป็นภาษาอังกฤษตามรูปแบบ Conventional Commits
- คงคำศัพท์เฉพาะหรือคำที่แปลแล้วแปลกเป็นภาษาอังกฤษ/ทับศัพท์ใน subject และ body เช่น `WebAuthn`, `passkey`, `TanStack Query`, `PostgreSQL`, `rate limiter`
- ถ้าผู้ใช้ขอ commit message ภาษาอังกฤษ ให้เขียน subject/body เป็นภาษาอังกฤษได้

## Workflow

1. อ่าน diff ก่อนเขียน commit message
   - ใช้ `git diff --staged` เป็นหลักเมื่อผู้ใช้กำลังเตรียม commit
   - ใช้ `git diff` เมื่อไม่มี staged changes หรือผู้ใช้ถามถึง working tree
   - ถ้าทั้ง staged และ unstaged changes มีผลต่อคำตอบ ให้อ่านทั้งคู่แล้วบอกให้ชัดว่า message ครอบคลุมส่วนไหน
2. หา intent หลักของการเปลี่ยนแปลง ไม่ต้องไล่สรุปทุกไฟล์
3. เลือก Conventional Commit type:
   - `feat`: เพิ่ม feature หรือความสามารถที่ผู้ใช้เห็นได้
   - `fix`: แก้ bug หรือพฤติกรรมที่ผิด
   - `docs`: เปลี่ยนเฉพาะ documentation
   - `style`: เปลี่ยน formatting เท่านั้น ไม่กระทบ behavior
   - `refactor`: เปลี่ยนโครงสร้าง code โดยไม่เพิ่ม feature และไม่แก้ bug
   - `perf`: ปรับ performance
   - `test`: เพิ่มหรือแก้ tests/test infrastructure
   - `build`: build system, dependencies, packaging
   - `ci`: CI configuration หรือ automation
   - `chore`: งาน maintenance ที่ไม่เข้ากลุ่มอื่น
   - `revert`: revert commit ก่อนหน้า
4. ใส่ scope เฉพาะเมื่อเห็นชัดและช่วยให้ message อ่านง่าย โดยใช้ lowercase kebab-case หรือ convention ของ project
5. เขียน subject ให้กระชับ อ่านเหมือนคำสั่งหรือผลลัพธ์ของ change และใช้ตัวพิมพ์เล็กหลัง type เมื่อเป็นภาษาอังกฤษ
6. ให้ subject กระชับ โดยทั่วไปประมาณ 50-72 ตัวอักษร
7. ใส่ body เฉพาะเมื่อ diff มีหลายประเด็นสำคัญ มี motivation ที่ไม่ obvious หรือมี behavior ที่ควรอธิบาย
8. ใส่ `BREAKING CHANGE:` ใน footer เฉพาะเมื่อ diff แสดงชัดว่ามี breaking API, schema, behavior หรือ compatibility change

## รูปแบบคำตอบ

สำหรับคำขอทั่วไป ให้ตอบเป็น commit message ที่แนะนำ 1 อันใน fenced `text` block:

```text
feat(auth): เพิ่มการเข้าสู่ระบบด้วย passkey
```

ถ้า body มีประโยชน์ ให้เว้นบรรทัดแล้วใส่ body ต่อ:

```text
feat(auth): เพิ่มการเข้าสู่ระบบด้วย passkey

เพิ่มขั้นตอนลงทะเบียนและเข้าสู่ระบบด้วย WebAuthn.
รองรับ fallback ไปยังรหัสผ่านเดิมเมื่ออุปกรณ์ไม่รองรับ passkey.
```

ถ้าผู้ใช้ขอตัวเลือก ให้เสนอ 2-3 แบบ พร้อมอธิบายสั้น ๆ เป็นภาษาไทยว่าแต่ละแบบเหมาะกับกรณีไหน

## Guardrails

- อย่าเดาการเปลี่ยนแปลงที่ไม่อยู่ใน diff
- อย่าเน้น generated files, formatting หรือชื่อไฟล์ เว้นแต่เป็นสาระหลักของ change
- เลือก commit message เดียวสำหรับ intent เดียวที่ชัดเจน ถ้า diff มีหลาย intent ที่ไม่เกี่ยวกัน ให้แนะนำ split commit และให้ message แยกกัน
- ถ้าไม่มี diff ให้ขอ diff จากผู้ใช้ หรือรัน git diff commands เมื่อเข้าถึง local repo ได้
- ถ้าผู้ใช้ขอภาษาไทย ให้ตอบคำอธิบายเป็นภาษาไทย และใช้คำศัพท์เฉพาะแบบทับศัพท์ตามความเหมาะสม
