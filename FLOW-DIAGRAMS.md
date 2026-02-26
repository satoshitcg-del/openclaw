# OpenClaw Flow Diagrams

แผนภาพแสดงการทำงานของ OpenClaw (Mermaid Diagrams)

---

## 1. System Architecture Overview

```mermaid
flowchart TB
    subgraph UserLayer["👤 User Layer"]
        UI1[Webchat]
        UI2[Discord]
        UI3[Telegram]
        UI4[CLI]
    end

    subgraph GatewayLayer["🌐 Gateway Layer"]
        GW[OpenClaw Gateway<br/>Node.js]
        RT[Router]
        SM[Session Manager]
    end

    subgraph AgentLayer["🤖 Agent Layer"]
        AC[Agent Core]
        TS[Tool Selector]
        SK[Skill Loader]
    end

    subgraph ToolLayer["🛠️ Tool Layer"]
        T1[read/write/exec]
        T2[browser]
        T3[web_search]
        T4[message]
        T5[Other Skills...]
    end

    subgraph MemoryLayer["🧠 Memory Layer"]
        M1[SOUL.md]
        M2[USER.md]
        M3[MEMORY.md]
        M4[Daily Logs]
        M5[Session History]
    end

    subgraph ModelLayer["🧩 AI Model Layer"]
        LLM1[OpenAI GPT]
        LLM2[Anthropic Claude]
        LLM3[Google Gemini]
        LLM4[Local Models<br/>Ollama/llama.cpp]
    end

    UI1 & UI2 & UI3 & UI4 --> GW
    GW --> RT
    RT --> SM
    SM --> AC
    
    AC --> TS
    TS --> SK
    SK --> T1 & T2 & T3 & T4 & T5
    
    AC --> M1 & M2 & M3 & M4 & M5
    
    AC <-->|API Call| LLM1
    AC <-->|API Call| LLM2
    AC <-->|API Call| LLM3
    AC <-->|Local| LLM4
```

---

## 2. Session Startup Flow

```mermaid
flowchart TD
    Start([🚀 Session Start]) --> CheckBoot{มี BOOTSTRAP.md?}
    
    CheckBoot -->|มี| ExecBoot[Execute BOOTSTRAP.md] --> DelBoot[Delete BOOTSTRAP.md]
    CheckBoot -->|ไม่มี| LoadSoul
    
    DelBoot --> LoadSoul[อ่าน SOUL.md]
    
    LoadSoul --> LoadUser[อ่าน USER.md]
    LoadUser --> LoadToday[อ่าน memory/วันนี้.md]
    LoadToday --> LoadYesterday[อ่าน memory/เมื่อวาน.md]
    
    LoadYesterday --> CheckMain{เป็น Main Session?}
    CheckMain -->|ใช่| LoadMemory[อ่าน MEMORY.md]
    CheckMain -->|ไม่| SkipMemory[Skip MEMORY.md]
    
    LoadMemory --> LoadProject[โหลด Project Context]
    SkipMemory --> LoadProject
    
    LoadProject --> LoadTools[โหลด TOOLS.md]
    LoadTools --> Ready([✅ Ready!])
```

---

## 3. Request Processing Flow

```mermaid
flowchart TD
    Request([👤 User Request]) --> LoadContext[โหลด Context]
    
    LoadContext --> Intent[วิเคราะห์ Intent]
    Intent --> SkillCheck{มี Skill<br/>ที่ตรงไหม?}
    
    SkillCheck -->|มี| LoadSkill[อ่าน SKILL.md]
    SkillCheck -->|ไม่มี| SelectTool[เลือก General Tools]
    
    LoadSkill --> ExecuteSkill[Execute Skill]
    SelectTool --> ExecuteSkill
    
    ExecuteSkill --> ToolCall{ต้องใช้<br/>Tool ไหม?}
    
    ToolCall -->|ใช่| CallTool[เรียก Tool]
    ToolCall -->|ไม่| Generate[สร้างคำตอบ]
    
    CallTool --> GetResult[รับผลลัพธ์]
    GetResult --> NeedMore{ต้องทำต่อ<br/>ไหม?}
    
    NeedMore -->|ใช่| CallTool
    NeedMore -->|ไม่| Generate
    
    Generate --> Response([💬 Response to User])
    
    Response --> Important{งานสำคัญ?}
    Important -->|ใช่| SaveMemory[บันทึก Memory]
    Important -->|ไม่| End([จบ])
    SaveMemory --> End
```

---

## 4. Memory System Flow

```mermaid
flowchart LR
    subgraph Input["📥 Input"]
        E1[เหตุการณ์]
        E2[งานสำเร็จ]
        E3[บทเรียน]
    end

    subgraph Daily["📝 Daily Log"]
        DL[memory/YYYY-MM-DD.md]
    end

    subgraph Curate["🎯 Curation"]
        Check{สำคัญ<br/>ระยะยาว?}
        Summarize[สรุป]
    end

    subgraph LongTerm["🧠 Long-term"]
        MM[MEMORY.md]
    end

    subgraph Archive["📦 Archive"]
        AR[memory/archive/]
    end

    E1 & E2 & E3 --> DL
    DL --> Check
    Check -->|ใช่| Summarize
    Check -->|ไม่| DL
    Summarize --> MM
    MM -->|เก่า/ใหญ่| AR
```

---

## 5. Context Management Flow

```mermaid
flowchart TD
    Start([เริ่ม Session]) --> LoadFiles[โหลดไฟล์ Context]
    
    LoadFiles --> SizeCheck{ขนาด Context<br/>เกิน 20K?}
    
    SizeCheck -->|ใช่| Reduce[ลดขนาด]
    SizeCheck -->|ไม่| Process[ประมวลผล]
    
    Reduce --> ArchiveOld[ย้ายไฟล์เก่าไป Archive]
    ArchiveOld --> Summarize[สรุป MEMORY.md]
    Summarize --> Process
    
    Process --> Request[รับ Request]
    Request --> AddContext[เพิ่ม Context]
    
    AddContext --> CheckAgain{เต็มไหม?}
    CheckAgain -->|ใช่| SuggestNew[แนะนำ /new]
    CheckAgain -->|ไม่| Continue[ดำเนินการต่อ]
    
    SuggestNew --> NewSession[/new Session]
    Continue --> End([ดำเนินการ])
```

---

## 6. Tool Selection Decision Tree

```mermaid
flowchart TD
    Request([คำขอ]) --> Type{ประเภทงาน?}
    
    Type -->|อ่านไฟล์| Read[read tool]
    Type -->|แก้ไขไฟล์| Edit{ขนาดการแก้ไข?}
    Type -->|สร้างไฟล์| Write[write tool]
    Type -->|คำสั่ง Shell| Exec[exec tool]
    Type -->|เว็บ| Web{ต้องการ<br/>อะไร?}
    Type -->|ส่งข้อความ| Msg[message tool]
    
    Edit -->|เล็กน้อย| EditTool[edit tool]
    Edit -->|ใหญ่/ใหม่| Write
    
    Web -->|ดึงข้อมูล| Fetch[web_fetch]
    Web -->|ค้นหา| Search[web_search]
    Web -->|ควบคุม browser| Browser[browser tool]
    
    Read & EditTool & Write & Exec & Fetch & Search & Browser & Msg --> Result([ดำเนินการ])
```

---

## 7. Skill System Architecture

```mermaid
flowchart TB
    subgraph SkillsDir["📁 Skills Directory"]
        S1[skill-a/]
        S2[skill-b/]
        S3[skill-c/]
    end

    subgraph SkillStructure["📂 Skill Structure"]
        SK1[SKILL.md]
        SK2[README.md]
        SK3[scripts/]
        SK4[assets/]
    end

    subgraph Agent["🤖 Agent"]
        Check{ตรงกับ<br/>คำขอ?}
        Load[โหลด SKILL.md]
        Exec[Execute]
    end

    S1 & S2 & S3 --> SK1 & SK2 & SK3 & SK4
    
    Request([คำขอ]) --> Check
    Check -->|ใช่| Load
    Check -->|ไม่| General[General Tools]
    
    Load --> Exec
    SK1 --> Load
    
    Exec --> Result([ผลลัพธ์])
    General --> Result
```

---

## 8. Cron & Heartbeat System

```mermaid
flowchart TD
    subgraph Schedule["⏰ Scheduling"]
        Cron[Cron Job<br/>เวลาที่กำหนด]
        HB[Heartbeat<br/>ตาม interval]
    end

    subgraph Check["🔍 Check Tasks"]
        C1[ตรวจสอบ Subagents]
        C2[ตรวจสอบ Memory]
        C3[ตรวจสอบสถานะ]
    end

    subgraph Action["⚡ Action"]
        Alert[แจ้งเตือน]
        Fix[แก้ไขอัตโนมัติ]
        Log[บันทึก Log]
    end

    Cron --> Check
    HB --> Check
    
    C1 & C2 & C3 --> NeedAction{ต้องทำ<br/>อะไร?}
    
    NeedAction -->|เร่งด่วน| Alert
    NeedAction -->|แก้ได้| Fix
    NeedAction -->|บันทึก| Log
    
    Alert --> Notify[แจ้ง User]
    Fix --> Report[รายงานผล]
    Log --> End([จบรอบ])
    Notify --> End
    Report --> End
```

---

## 9. Sub-agent Orchestration

```mermaid
flowchart TD
    Main([Main Agent]) --> Complex{งานซับซ้อน?}
    
    Complex -->|ใช่| Split[แยกงานย่อย]
    Complex -->|ไม่| DoSelf[ทำเอง]
    
    Split --> Spawn1[Spawn Agent 1]
    Split --> Spawn2[Spawn Agent 2]
    Split --> Spawn3[Spawn Agent 3]
    
    Spawn1 --> Task1[งาน A]
    Spawn2 --> Task2[งาน B]
    Spawn3 --> Task3[งาน C]
    
    Task1 --> Collect[รวมผลลัพธ์]
    Task2 --> Collect
    Task3 --> Collect
    
    DoSelf --> Single[ทำงานเดี่ยว]
    Single --> Result
    
    Collect --> Aggregate[สรุปรวม]
    Aggregate --> Result([ส่งผลลัพธ์])
```

---

## 10. Error Handling Flow

```mermaid
flowchart TD
    Error[❌ Error Occurred] --> Classify{ประเภท?}
    
    Classify -->|Tool Error| ToolFix[ลอง Tool อื่น]
    Classify -->|Permission| Ask[ถาม User]
    Classify -->|Not Found| Search[ค้นหาเพิ่ม]
    Classify -->|Timeout| Retry{ลองใหม่?}
    
    ToolFix --> Success{สำเร็จ?}
    Search --> Success
    
    Success -->|ใช่| Continue[ดำเนินการต่อ]
    Success -->|ไม่| Escalate[ยกระดับ]
    
    Retry -->|ใช่| RetryAction[ลองอีกครั้ง]
    Retry -->|ไม่| Escalate
    
    Ask --> UserResp[รอคำตอบ]
    UserResp --> Continue
    
    RetryAction --> Success
    Escalate --> Report[รายงาน User]
    Continue --> End([จบ])
    Report --> End
```

---

*แผนภาพเหล่านี้สร้างด้วย Mermaid syntax*  
*GitHub รองรับการ render Mermaid diagrams โดยอัตโนมัติ*  
*สร้างเมื่อ: 2026-02-26 โดย เสี่ยวทู่ 🐰*
