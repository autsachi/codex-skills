# Codex Skills

คลัง reusable skills สำหรับช่วยให้ Codex ทำงานตาม workflow เดิมได้สม่ำเสมอ แต่ละ skill เรียกใช้โดยตรงด้วย `$skill-name` หรือให้ Codex เลือกใช้อัตโนมัติเมื่อคำขอตรงกับ `description`

## ติดตั้ง

ใน Codex task ให้เรียก `$skill-installer` แล้วระบุชื่อ skill และ GitHub URL:

```text
$skill-installer ติดตั้ง <skill-name> จาก
https://github.com/autsachi/codex-skills/tree/main/skills/<skill-name>
```

หลังติดตั้ง skill จะพร้อมใช้ใน turn ถัดไป หากยังไม่ปรากฏให้เริ่ม Codex task ใหม่

## Skills

| Skill                                                | ใช้สำหรับ                                                                                    |
| ---------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| [`commit-message`](skills/commit-message/)           | อ่าน diff แล้วเขียน Conventional Commit ภาษาไทย พร้อมแนะนำให้แยก commit เมื่อมีหลาย intent   |
| [`git-deliver-work`](skills/git-deliver-work/)       | ตรวจ diff และ checks, stage เฉพาะไฟล์ที่เกี่ยวข้อง แล้ว commit หรือ push อย่างปลอดภัย        |
| [`git-flow-release`](skills/git-flow-release/)       | เตรียม ปิด และ publish Git Flow release โดยค้นหา convention ของแต่ละ repository              |
| [`repo-next-task`](skills/repo-next-task/)           | ตรวจ repository, roadmap, TODO, issues และ recent commits เพื่อแนะนำงานถัดไปแบบ read-only    |
| [`responsive-ux-audit`](skills/responsive-ux-audit/) | ตรวจและแก้ responsive UX บน mobile, tablet, desktop, orientation, keyboard และ safe area     |
| [`scrutinize`](skills/scrutinize/)                   | review plan, PR, diff หรือ design แบบ end-to-end และรายงาน findings จากหลักฐานจริง           |
| [`post-mortem`](skills/post-mortem/)                 | เขียน engineering post-mortem หรือ RCA จาก bug ที่ยืนยัน root cause, fix และ validation แล้ว |
| [`management-talk`](skills/management-talk/)         | แปลงข้อมูลเชิงวิศวกรรมเป็น status update สำหรับ PM หรือ leadership                           |

## ที่มาและเครดิต

แนวคิดของ `scrutinize`, `post-mortem` และ `management-talk` ได้รับแรงบันดาลใจจาก [thananon/9arm-skills](https://github.com/thananon/9arm-skills):

- [scrutinize ต้นฉบับ](https://github.com/thananon/9arm-skills/blob/main/skills/engineering/scrutinize/SKILL.md)
- [post-mortem ต้นฉบับ](https://github.com/thananon/9arm-skills/blob/main/skills/engineering/post-mortem/SKILL.md)
- [management-talk ต้นฉบับ](https://github.com/thananon/9arm-skills/blob/main/skills/productivity/management-talk/SKILL.md)

เนื้อหาใน repository นี้เรียบเรียงใหม่สำหรับ Codex และ personal workflow ของผู้ดูแล ไม่ใช่สำเนาแบบ verbatim หรือ upstream mirror

## License

เผยแพร่ภายใต้ [MIT License](LICENSE)
