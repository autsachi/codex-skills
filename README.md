# Codex Skills

Personal, reusable Agent Skills for practical software development workflows. Built and tested with Codex.

คลัง skills ส่วนตัวที่คัดเลือกแล้วสำหรับช่วยให้ Codex ทำงานด้าน software development ตาม workflow ที่สม่ำเสมอ ตั้งแต่เขียน commit message, ส่งมอบ Git changes, เลือกงานถัดไป, ตรวจ responsive UX ไปจนถึง review, post-mortem และการสื่อสารกับ leadership

โครงสร้างหลักของแต่ละ skill อ้างอิง [Agent Skills open standard](https://agentskills.io/) แต่ repository นี้ออกแบบและทดสอบกับ Codex เป็นหลัก ไฟล์ `agents/openai.yaml` เป็น metadata สำหรับ OpenAI surfaces ส่วนการใช้งานกับ agent อื่นถือเป็น best effort

## ติดตั้ง

ใน Codex task ให้เรียก `$skill-installer` พร้อมชื่อ skill และ GitHub URL:

```text
$skill-installer ติดตั้ง <skill-name> จาก
https://github.com/autsachi/codex-skills/tree/main/skills/<skill-name>
```

ตัวอย่าง:

```text
$skill-installer ติดตั้ง commit-message จาก
https://github.com/autsachi/codex-skills/tree/main/skills/commit-message
```

หลังติดตั้ง skill จะพร้อมใช้ใน turn ถัดไป หากยังไม่ปรากฏให้เริ่ม Codex task ใหม่

## Skills

| Skill                                                | ช่วยทำอะไร                                                                                     | ใช้เมื่อ                                                                                  |
| ---------------------------------------------------- | ---------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| [`commit-message`](skills/commit-message/)           | อ่าน diff แล้วเขียน Conventional Commit ภาษาไทย โดยรักษาศัพท์เทคนิคที่ควรคงเป็นภาษาอังกฤษ      | ต้องการ commit title/body, เลือก `type(scope):` หรือตรวจว่า change ควรแยก commits หรือไม่ |
| [`git-deliver-work`](skills/git-deliver-work/)       | ตรวจ diff และ checks, stage เฉพาะไฟล์ที่เกี่ยวข้อง แล้ว commit หรือ push อย่างปลอดภัย          | สั่ง commit, push หรือ commit and push โดยไม่เปิด pull request                            |
| [`repo-next-task`](skills/repo-next-task/)           | ตรวจสถานะ repository และหลักฐานจาก roadmap, TODO, issues และ recent commits เพื่อแนะนำงานถัดไป | ถามว่า “ควรทำอะไรต่อ” หรือต้องการจัดลำดับ backlog แบบ read-only                           |
| [`responsive-ux-audit`](skills/responsive-ux-audit/) | ตรวจและแก้ responsive UX บน mobile, tablet, desktop, orientation, keyboard และ safe area       | พบ overflow, layout แตก, touch target เล็ก หรือต้องการ cross-device audit                 |
| [`scrutinize`](skills/scrutinize/)                   | review plan, PR, diff หรือ design แบบ end-to-end และรายงาน findings จากหลักฐานจริง             | ต้องการ second opinion, sanity check หรือประเมินความเสี่ยงก่อน merge                      |
| [`post-mortem`](skills/post-mortem/)                 | เขียน engineering post-mortem หรือ RCA จาก bug ที่แก้และยืนยัน root cause แล้ว                 | ต้องการบันทึก symptom, root cause, fix, validation และสาเหตุที่ปัญหาหลุดรอด               |
| [`management-talk`](skills/management-talk/)         | แปลงข้อมูลเชิงวิศวกรรมเป็นข้อความที่ PM หรือ leadership ใช้ตัดสินใจได้                         | ต้องการ status update สำหรับ JIRA, Slack, standup, email หรือ meeting                     |

## ตัวอย่างการเรียกใช้

```text
$commit-message อ่าน staged diff แล้วเสนอ Conventional Commit ภาษาไทยหนึ่งชุด

$git-deliver-work ตรวจ change ชุดนี้แล้ว commit และ push branch ปัจจุบัน

$repo-next-task ตรวจ repo นี้แล้วแนะนำงานถัดไปที่ช่วยปิด milestone ปัจจุบัน

$responsive-ux-audit audit หน้า checkout บน mobile และ tablet แบบ read-only

$scrutinize review diff เทียบกับ main โดยเน้น correctness และ regression risk

$post-mortem สร้าง RCA จาก failing test, fix และผล validation ที่แนบมา

$management-talk แปลงสรุปทางวิศวกรรมนี้เป็น Slack update สำหรับ PM
```

## การดูแล repository

Repository นี้เป็น curated public collection จาก private workspace ของผู้ดูแล แต่ละ skill จะถูกนำมาเผยแพร่เมื่อ instructions, metadata และขอบเขตความปลอดภัยพร้อมใช้งานสาธารณะ จึงไม่มีการ sync อัตโนมัติระหว่างสอง repositories

## ที่มาและเครดิต

แนวคิดของ `scrutinize`, `post-mortem` และ `management-talk` ได้รับแรงบันดาลใจจาก [thananon/9arm-skills](https://github.com/thananon/9arm-skills):

- [scrutinize ต้นฉบับ](https://github.com/thananon/9arm-skills/blob/main/skills/engineering/scrutinize/SKILL.md)
- [post-mortem ต้นฉบับ](https://github.com/thananon/9arm-skills/blob/main/skills/engineering/post-mortem/SKILL.md)
- [management-talk ต้นฉบับ](https://github.com/thananon/9arm-skills/blob/main/skills/productivity/management-talk/SKILL.md)

เนื้อหาใน repository นี้เรียบเรียงใหม่สำหรับ Codex และ personal workflow ของผู้ดูแล ไม่ใช่สำเนาแบบ verbatim และไม่ใช่ upstream mirror

## License

เผยแพร่ภายใต้ [MIT License](LICENSE)
