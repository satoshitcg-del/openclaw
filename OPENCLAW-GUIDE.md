# OpenClaw Architecture & Memory System

เอกสารอธิบายการทำงานของ OpenClaw — สร้างโดย เสี่ยวทู่ 🐰

---

## 📋 สารบัญ

1. [OpenClaw คืออะไร](#1-openclaw-คืออะไร)
2. [สถาปัตยกรรมระบบ](#2-สถาปัตยกรรมระบบ)
3. [ระบบความจำ (Memory System)](#3-ระบบความจำ-memory-system)
4. [การทำงานของ Agent](#4-การทำงานของ-agent)
5. [วิธีทำให้ OpenClaw เก่งขึ้น](#5-วิธีทำให้-openclaw-เก่งขึ้น)
6. [Best Practices](#6-best-practices)

---

## 1. OpenClaw คืออะไร

**OpenClaw** = AI Agent Framework ที่ทำงานบนเครื่องผู้ใช้ (Local-First AI Assistant)

### จุดเด่น
- 🏠 **Local-First** — ทำงานบนเครื่องผู้ใช้ ไม่ต้องพึ่ง cloud ทั้งหมด
- 🔧 **Tool Integration** — เชื่อมต่อกับเครื่องมือต่างๆ (Browser, File System, Git, etc.)
- 🧠 **Memory Persistence** — จดจำบริบทระหว่าง session
- 👤 **Persona System** — ปรับแต่งบุคลิก AI ได้

---

## 2. สถาปัตยกรรมระบบ

```
┌─────────────────────────────────────────────────────────┐
│                    User Interface                       │
│         (Webchat / Discord / Telegram / CLI)            │
└────────────────────┬────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────┐
│              OpenClaw Gateway (Node.js)                 │
│  • Message routing  • Session management  • Tool calls  │
└────────────────────┬────────────────────────────────────┘
                     │
        ┌────────────┼────────────┐
        │            │            │
   ┌────▼────┐  ┌────▼────┐  ┌────▼────┐
   │  Agent  │  │  Skills │  │ Memory  │
   │  Core   │  │ (Tools) │  │ System  │
   └─────────┘  └─────────┘  └─────────┘
        │            │            │
   ┌────▼────────────▼────────────▼────┐
   │      AI Model (LLM Provider)       │
   │  (OpenAI / Anthropic / Google /    │
   │   Local Models via Ollama/llama)   │
   └────────────────────────────────────┘
```

### ส่วนประกอบหลัก

| ส่วน | หน้าที่ | ตัวอย่าง |
|------|--------|---------|
| **Gateway** | จัดการการเชื่อมต่อ | รับ message, ส่งต่อให้ agent |
| **Agent** | ประมวลผลคำขอ | ตัดสินใจใช้ tools, สร้างคำตอบ |
| **Skills** | เครื่องมือเฉพาะทาง | Browser, Git, File operations |
| **Memory** | จัดเก็บข้อมูล | Context files, Session history |

---

## 3. ระบบความจำ (Memory System)

### 3.1 ประเภทของความจำ

```
┌─────────────────────────────────────────────────────────┐
│                   Memory Hierarchy                      │
├─────────────────────────────────────────────────────────┤
│  🧠 Long-term Memory (MEMORY.md)                        │
│     → ข้อมูลสำคัญที่สรุปแล้ว (สูงสุด ~20KB)            │
├─────────────────────────────────────────────────────────┤
│  📝 Daily Memory (memory/YYYY-MM-DD.md)                 │
│     → Logs ของแต่ละวัน (อ่านทุก session)                │
├─────────────────────────────────────────────────────────┤
│  ⚡ Session Context (Project Context)                   │
│     → ไฟล์ที่ inject เข้า prompt ทุกครั้ง               │
│     → AGENTS.md, SOUL.md, USER.md, TOOLS.md             │
├─────────────────────────────────────────────────────────┤
│  💾 Session History                                     │
│     → บทสนทนาใน session ปัจจุบัน                        │
└─────────────────────────────────────────────────────────┘
```

### 3.2 การโหลดความจำ (Session Startup)

ทุกครั้งที่เริ่ม session ใหม่ OpenClaw จะ:

```python
1. อ่าน SOUL.md          → โหลดบุคลิก/ค่านิยม
2. อ่าน USER.md          → โหลดข้อมูลผู้ใช้
3. อ่าน memory/วันนี้    → โหลด context ล่าสุด
4. อ่าน memory/เมื่อวาน  → โหลด context วันก่อน
5. (ถ้าเป็น main session) → อ่าน MEMORY.md
```

### 3.3 การเขียนความจำ

**เมื่อไหร่ควรเขียน:**
- ✅ เสร็จโปรเจกต์สำคัญ
- ✅ เรียนรู้บทเรียนใหม่
- ✅ ผู้ใช้บอกให้ "จำไว้"
- ✅ เกิด error / แก้ปัญหาสำเร็จ

**หลักการเขียน:**
```markdown
# Memory Log - YYYY-MM-DD

## งานที่ทำวันนี้
- [โปรเจกต์] สิ่งที่ทำ + ผลลัพธ์

## บทเรียนที่ได้รับ
- สิ่งที่เรียนรู้

## งานพรุ่งนี้
- สิ่งที่ต้องทำต่อ
```

---

## 4. การทำงานของ Agent

### 4.1 ขั้นตอนการประมวลผลคำขอ

```
User Request
      │
      ▼
┌─────────────┐
│ 1. Context  │ ← โหลดไฟล์ความจำ
│   Loading   │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│ 2. Intent   │ ← วิเคราะห์ว่าผู้ใช้ต้องการอะไร
│  Analysis   │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│ 3. Tool     │ ← เลือกเครื่องมือที่เหมาะสม
│  Selection  │   (read/write/exec/browser/etc.)
└──────┬──────┘
       │
       ▼
┌─────────────┐
│ 4. Tool     │ ← เรียกใช้เครื่องมือ
│  Execution  │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│ 5. Response │ ← สร้างคำตอบจากผลลัพธ์
│  Generation │
└──────┬──────┘
       │
       ▼
   Response
```

### 4.2 Skill Selection Logic

```python
if task_matches_skill:
    read(SKILL.md)  # อ่านคู่มือ skill
    execute(skill)  # ทำตามขั้นตอน
else:
    use_general_tools()  # ใช้ tools ทั่วไป
```

**กฎการเลือก Skill:**
1. ถ้ามี skill เดียวตรง → ใช้เลย
2. ถ้ามีหลาย skill → เลือกอันที่เฉพาะเจาะจงที่สุด
3. ถ้าไม่มี skill → ใช้ tools ทั่วไป

---

## 5. วิธีทำให้ OpenClaw เก่งขึ้น

### 5.1 ปรับปรุงความจำ

| วิธี | ผลลัพธ์ | ตัวอย่าง |
|------|---------|---------|
| **สรุปประจำ** | ลด noise | สรุป memory รายสัปดาห์ |
| **ใช้ Tags** | หาเจอเร็ว | `[โปรเจกต์สำคัญ]`, `[บทเรียน]` |
| **Link ไฟล์** | ตามหาได้ | "ดูรายละเอียดที่ `file.md`" |
| **Archive เก่า** | ลดขนาด | ย้ายไฟล์เก่าไป `archive/` |

### 5.2 ปรับปรุง Personality (SOUL.md)

```markdown
# SOUL.md - Who You Are

## Core Truths
- Be genuinely helpful (ไม่ต้องพูดมาก)
- Have opinions (มีความคิดเห็น)
- Be resourceful (หาคำตอบก่อนถาม)

## Vibe
- สไตล์การสื่อสารที่ชอบ
- การใช้ emoji / ภาษาที่ชอบ
- ระดับ formality

## Boundaries
- อะไรทำได้ / ทำไม่ได้
- อะไรต้องถามก่อน
```

### 5.3 สร้าง Skills ใหม่

```
skills/
└── your-skill/
    ├── SKILL.md          # คู่มือการใช้
    ├── README.md         # อธิบายโดยรวม
    └── (scripts/assets)  # ไฟล์ประกอบ
```

**SKILL.md Template:**
```markdown
# Skill Name

## When to Use
เมื่อไหร่ควรใช้ skill นี้

## Tools Provided
- tool1: คำอธิบาย
- tool2: คำอธิบาย

## Usage
ขั้นตอนการใช้งาน

## Examples
ตัวอย่างการใช้
```

### 5.4 ปรับปรุง Context Management

**ปัญหา:** Context limit (~20K ตัวอักษร)

**วิธีแก้:**
1. **สรุปสั้น** — ใช้ bullet points
2. **แยกไฟล์** — เนื้อหายาว → หลายไฟล์
3. **อ้างอิง** — "ดูรายละเอียดที่ `details.md`"
4. **ลบซ้ำ** — ไม่เขียนข้อมูลซ้ำ

---

## 6. Best Practices

### สำหรับผู้ใช้ (User)

| ✅ ทำ | ❌ อย่าทำ |
|------|---------|
| บอกให้ "จำไว้" เมื่อสำคัญ | ปล่อยให้ agent เดาเอง |
| อ่าน/แก้ไข memory เองได้ | ลบ memory ทั้งหมด |
| ใช้ `/new` เริ่ม session ใหม่ | พิมพ์ต่อเนื่องจน context เต็ม |
| ตรวจสอบ MEMORY.md เป็นระยะ | ปล่อยให้ใหญ่เกิน 20KB |

### สำหรับ Agent (เสี่ยวทู่)

| ✅ ทำ | ❌ อย่าทำ |
|------|---------|
| อ่าน memory ทุกครั้งเริ่ม session | ถามซ้ำสิ่งที่จดไว้แล้ว |
| เขียน memory เมื่อเสร็จงานสำคัญ | ลืมบันทึกบทเรียน |
| ใช้ skills เมื่อมี | ทำเองทั้งหมด |
| บอก user เมื่อแก้ไขไฟล์สำคัญ | แก้ไขโดยไม่บอก |

---

## อ้างอิง

- [OpenClaw Docs](https://docs.openclaw.ai)
- [OpenClaw GitHub](https://github.com/openclaw/openclaw)
- [Community Discord](https://discord.com/invite/clawd)

---

*เอกสารนี้สร้างเมื่อ: 2026-02-26*  
*สร้างโดย: เสี่ยวทู่ 🐰*
