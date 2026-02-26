# OpenClaw Memory System Deep Dive

คู่มือลึกเกี่ยวกับระบบความจำของ OpenClaw

---

## โครงสร้างไฟล์ความจำ

```
workspace/
├── AGENTS.md              # กฎการทำงานของ agent
├── SOUL.md                # บุคลิกของ agent
├── USER.md                # ข้อมูลผู้ใช้
├── TOOLS.md               # ข้อมูลเครื่องมือเฉพาะ
├── MEMORY.md              # ความจำระยะยาว (โหลดเฉพาะ main session)
├── BOOTSTRAP.md           # คำสั่งเริ่มต้น (ถ้ามี → ทำแล้วลบ)
├── HEARTBEAT.md           # งานที่ตรวจสอบเป็นระยะ
│
└── memory/                # โฟลเดอร์บันทึกรายวัน
    ├── YYYY-MM-DD.md      # บันทึกของแต่ละวัน
    └── archive/           # ไฟล์เก่าที่ archive
        └── README.md      # index ของ archive
```

---

## การทำงานของแต่ละไฟล์

### AGENTS.md
**โหลดเมื่อ:** ทุก session  
**เนื้อหา:** กฎการทำงาน, ขั้นตอน session startup, memory management

```markdown
## Session Startup
1. อ่าน SOUL.md
2. อ่าน USER.md
3. อ่าน memory/วันนี้
4. อ่าน memory/เมื่อวาน
```

### SOUL.md
**โหลดเมื่อ:** ทุก session  
**เนื้อหา:** บุคลิก, สไตล์การสื่อสาร, ค่านิยม

```markdown
## Core Truths
- Be genuinely helpful
- Have opinions
- Remember you're a guest
```

### USER.md
**โหลดเมื่อ:** ทุก session  
**เนื้อหา:** ข้อมูลผู้ใช้ที่ agent ควรรู้

```markdown
- Name: อิจิ
- Timezone: Asia/Bangkok
- Projects: QA Learning Hub
```

### MEMORY.md
**โหลดเมื่อ:** เฉพาะ main session (direct chat)  
**เนื้อหา:** ความจำระยะยาวที่สรุปแล้ว

**ข้อควรระวัง:**
- ขนาดไม่ควรเกิน ~20KB
- ถ้าใหญ่เกิน → ย้ายไป archive

### memory/YYYY-MM-DD.md
**โหลดเมื่อ:** ทุก session (วันนี้ + เมื่อวาน)  
**เนื้อหา:** logs รายวัน, งานที่ทำ, บทเรียน

---

## Context Limit & Management

### ข้อจำกัด
- **Context limit:** ~20,000 ตัวอักษรต่อไฟล์
- **ผลกระทบ:** ถ้าเกิน → ข้อมูลถูก truncate

### วิธีจัดการ

#### 1. สรุปให้สั้น
```markdown
## งานที่ทำ
- ✅ QA Learning Hub: 29 หน้า (ดูรายละเอียดใน archive/2026-02-24.md)
```

#### 2. แยกไฟล์
```
memory/
├── 2026-02-24.md          (สรุปสั้น)
└── archive/
    └── 2026-02-24-full.md  (รายละเอียดเต็ม)
```

#### 3. ใช้ Archive
```powershell
# ย้ายไฟล์เก่าไป archive
Move-Item memory/2026-02-24.md memory/archive/
```

---

## Memory Lifecycle

```
┌─────────────────────────────────────────────────────────┐
│                    Memory Lifecycle                     │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  Event ──────► Daily Log ──────► MEMORY.md ──────► Archive
│    │              │                │              │
│    │              │                │              │
│    ▼              ▼                ▼              ▼
│  งานสำเร็จ    บันทึกรายวัน    สรุประยะยาว    เก็บถาวร
│  เรียนรู้ใหม่   (raw)          (curated)      (reference)
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### 1. Daily Log (Raw)
บันทึกทุกอย่างที่เกิดขึ้นในวันนั้น

### 2. MEMORY.md (Curated)
สรุปสิ่งสำคัญที่ควรจำระยะยาว

### 3. Archive (Reference)
ไฟล์เก่าที่อาจต้องกลับมาดู

---

## Tips & Tricks

### การค้นหาใน Memory
```powershell
# ค้นหาคำใน memory
Select-String -Path memory/*.md -Pattern "QA Learning Hub"
```

### การ Backup Memory
```bash
# Commit ไป git
git add memory/
git commit -m "Update memory logs"
```

### การลบ Memory เก่า
```powershell
# ย้ายไฟล์เก่ากว่า 30 วันไป archive
Get-ChildItem memory/*.md | Where-Object { $_.LastWriteTime -lt (Get-Date).AddDays(-30) }
```

---

## Common Patterns

### Pattern 1: Project Completion
```markdown
## โปรเจกต์ XXX (YYYY-MM-DD) ✅
- **สถานะ:** เสร็จสมบูรณ์
- **ไฟล์:** path/to/files
- **รายละเอียด:** ดูใน memory/YYYY-MM-DD.md
```

### Pattern 2: Lesson Learned
```markdown
## บทเรียน: [หัวข้อ]
- **ปัญหา:** ...
- **วิธีแก้:** ...
- **อ้างอิง:** memory/วันที่
```

### Pattern 3: Decision Log
```markdown
## การตัดสินใจ: [เรื่อง]
- **วันที่:** YYYY-MM-DD
- **ตัดสินใจ:** ...
- **เหตุผล:** ...
```

---

## Troubleshooting

### ปัญหา: MEMORY.md ใหญ่เกินไป
**แก้ไข:**
1. อ่านทั้งหมด
2. สรุปให้สั้น
3. ย้ายรายละเอียดไป archive

### ปัญหา: Agent ไม่จำเหตุการณ์เก่า
**แก้ไข:**
1. ตรวจสอบว่า memory วันนั้นมีไหม
2. ถ้าไม่มี → อ่านจาก archive
3. อัปเดต MEMORY.md

### ปัญหา: Context เต็มเร็วเกินไป
**แก้ไข:**
1. ลดขนาด MEMORY.md
2. ใช้ `/new` บ่อยขึ้น
3. Archive ไฟล์เก่า

---

*เอกสารนี้สร้างเมื่อ: 2026-02-26*
