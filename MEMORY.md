# MEMORY.md - Long-Term Memory

ความทรงจำระยะยาวของ เสี่ยวทู่ เกี่ยวกับ อิจิ

---

## ข้อมูลพื้นฐาน

- **ชื่อ:** อิจิ
- **สถานที่:** ประเทศไทย (Asia/Bangkok)
- **เรียกว่า:** อิจิ

---

## โปรเจกต์ที่ทำอยู่

### QA Learning Hub (2026-02-24) 🎓
เว็บไซต์เรียนรู้ QA แบบครบวงจร — 29 หน้า ครอบคลุมทุกหัวข้อจาก roadmap.sh/qa

**ไฟล์หลัก:**
- `qa-learning-hub/index.html` — หน้าหลัก + แผนการเรียน
- `qa-learning-hub/topics/*.html` — 29 หน้าบทเรียน
- `qa-learning-hub/css/style.css, topic.css` — สไตล์
- `qa-learning-hub/js/main.js, topic.js` — JavaScript

**หัวข้อที่ครอบคลุม:**
1. **Basics (8 หน้า):** Internet, HTTP, What is QA, Testing Types, SDLC/STLC, V-Model, TDD/BDD, Git
2. **Manual (7 หน้า):** Test Scenarios, Testing Techniques, Bug Reporting, Test Documentation, Risk-Based Testing, Exploratory Testing, Compatibility Testing
3. **Automation (5 หน้า):** Selenium, Cypress, Playwright, API Testing, CI/CD Integration
4. **Non-Functional (4 หน้า):** Performance, Security, Accessibility, Usability
5. **Advanced (5 หน้า):** Mobile Testing, Database Testing, Agile/Scrum, Test Management, Test Oracles

---

### QA Exam Preparation Project (2026-02-19) 🎓
โปรเจกต์สร้างคู่มือเตรียมสอบ QA/Tester แบบครบวงจร — แผนการอ่าน 7 วัน + ข้อสอบ 80 ข้อ

**ไฟล์หลัก:**
| ไฟล์ | ขนาด | รายละเอียด |
|------|------|------------|
| `QA-Exam-Preparation-Easy-Polished.html` | 221 KB | คู่มือหลัก อธิบายละเอียด |
| `QA-Exam-Preparation-Easy.html` | 205 KB | เวอร์ชันกลาง |
| `README-QA-Exam-Preparation.md` | 6 KB | คู่มือการใช้งาน |

---

## สถานะการเรียนรู้

✅ **Review Cycles 1-15:** ทบทวนเครื่องมือหลัก 10 ตัวครบ 15 รอบ
✅ **QA Learning Hub:** สร้างเว็บครบ 29 หน้า (2026-02-24)

### เครื่องมือที่เชี่ยวชาญ (10 ตัวหลัก)
1. GitHub CLI (`gh`) — PR, Issues, CI/CD checks
2. Coding Agents (Codex, Claude) — PTY mode, background tasks
3. Oracle (File Bundling) — Browser mode, dry-run
4. API QA Tester — Browser DevTools, endpoint discovery
5. Billing Flow Tester — Payment workflows, status transitions
6. Rate Limit Analyzer — 4 Tiers (Critical/Sensitive/Standard/High)
7. Browser Automation — CDP, snapshot, actions
8. Cron & Sessions — Scheduling, isolated sessions
9. Web Search/Fetch — Brave API, markdown extraction
10. Media Tools — TTS, PDF, Video frames, Summarize

---

## การตั้งค่า OpenClaw

### Browser Configuration
- **Profile:** `openclaw` (isolated managed browser)
- **Port:** 18800
- **ไม่ต้อง Chrome Extension** — ใช้ CDP โดยตรง

### Cron Jobs
- **Monitor:** `qa-learning-hub-monitor` — ตรวจสอบทุก 5 นาที
- **Status:** กำลังทำงานปกติ

---

## บทบาทที่ต้องการ

- โปรแกรมเมอร์ (เขียนโค้ด แก้บั๊ก รีวิว code)
- เทสเตอร์ (เขียนเทส หา edge cases ตรวจสอบคุณภาพ)
- **อนุญาตใช้ token เต็มที่** — ไม่ต้องประหยัด เอาให้ดีที่สุด

---

*ไฟล์นี้อัปเดตล่าสุด: 2026-02-24*
*รายละเอียดการเรียนรู้แบบเต็ม: ดูใน `memory/archive/`*
