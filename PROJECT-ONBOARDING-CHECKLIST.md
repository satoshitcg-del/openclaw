# Project Onboarding Checklist for QA/Testing

เช็คลิสต์ข้อมูลที่ต้องมีเพื่อเข้าใจระบบเว็บแอปพลิเคชันทั้งหมด

---

## 📋 1. ข้อมูลระบบเบื้องต้น (System Overview)

### 1.1 Architecture & Infrastructure
```
☐ High-level Architecture Diagram
☐ Tech Stack (Frontend/Backend/Database)
☐ Infrastructure Diagram (Servers, Cloud, CDN)
☐ Third-party Integrations (APIs, Services)
☐ Data Flow Diagram
```

### 1.2 Technology Stack
| ส่วน | ข้อมูลที่ต้องการ | ตัวอย่าง |
|------|-----------------|---------|
| **Frontend** | Framework, State Management | React + Redux, Vue + Pinia |
| **Backend** | Framework, Language | Node.js/Express, Python/FastAPI |
| **Database** | Type, Version, ORM | PostgreSQL 15, Prisma |
| **Cache** | Redis, Memcached | Redis 7 |
| **Queue** | Message Queue | RabbitMQ, Kafka |
| **Storage** | File Storage | AWS S3, Local |
| **Auth** | Authentication | JWT, OAuth2, Session |

---

## 📁 2. เอกสารโปรเจกต์ (Project Documentation)

### 2.1 Functional Requirements
```
☐ Product Requirements Document (PRD)
☐ User Stories / Use Cases
☐ Feature Specifications
☐ User Flows / Journey Maps
☐ Wireframes / Mockups / UI Designs
☐ Acceptance Criteria ของแต่ละ Feature
```

### 2.2 API Documentation
```
☐ API Endpoints List (REST/GraphQL/gRPC)
☐ API Specifications (OpenAPI/Swagger)
☐ Request/Response Examples
☐ Error Codes & Handling
☐ Rate Limiting Rules
☐ Authentication & Authorization
```

### 2.3 Database Documentation
```
☐ Entity Relationship Diagram (ERD)
☐ Database Schema
☐ Migration History
☐ Data Dictionary (ความหมายของแต่ละ field)
☐ Indexing Strategy
```

---

## 🔧 3. โค้ดและการพัฒนา (Code & Development)

### 3.1 Repository Structure
```
☐ Git Repository Access
☐ Branching Strategy (Git Flow, Trunk-based)
☐ Folder Structure Conventions
☐ Code Style Guide / Linting Rules
☐ CI/CD Pipeline Configuration
```

### 3.2 Code Organization
| ส่วน | ที่อยู่ | รูปแบบ |
|------|--------|--------|
| Frontend Source | `/frontend`, `/src` | Components, Pages, Hooks |
| Backend Source | `/backend`, `/api` | Controllers, Services, Models |
| Shared Types | `/types`, `/shared` | Interfaces, DTOs |
| Tests | `/tests`, `*.test.*` | Unit, Integration, E2E |
| Configs | `/config`, `.env.*` | Environment variables |

### 3.3 Key Files ที่ต้องรู้จัก
```
☐ Entry Points (main.ts, index.js, App.tsx)
☐ Routing Configuration
☐ State Management Setup
☐ Database Connection/Config
☐ Middleware Configuration
☐ Environment Variables Template (.env.example)
```

---

## 🧪 4. สิ่งที่ต้องมีสำหรับการทดสอบ (Testing Artifacts)

### 4.1 Test Infrastructure
```
☐ Test Environments (Dev/Staging/Production)
☐ Test Data Sets (Seeds, Fixtures)
☐ Mock Services / Test Doubles
☐ Test User Accounts
☐ Browser/Device Requirements
```

### 4.2 Existing Tests
```
☐ Unit Test Coverage Report
☐ Integration Test Suites
☐ E2E Test Scenarios
☐ Performance Test Baselines
☐ Security Test Results
```

---

## 🔍 5. สำหรับ Impact Analysis (การประเมินผลกระทบ)

### 5.1 Dependency Mapping
```
☐ Module Dependency Graph
☐ Service Dependencies (Internal/External)
☐ Database Table Relationships
☐ API Consumer Dependencies
☐ Shared Components/Library Usage
```

### 5.2 Change Impact Matrix
| Component | Dependencies | Risk Level | Affected Areas |
|-----------|-------------|------------|----------------|
| Auth Service | User API, Session Store | High | Login, Register, Password Reset |
| Payment API | Stripe, Database | Critical | Checkout, Billing, Refunds |
| Search Feature | Elasticsearch | Medium | Search, Filters, Sorting |

### 5.3 Critical Paths
```
☐ User Registration Flow
☐ Login/Authentication Flow
☐ Payment/Checkout Flow
☐ Core Business Logic Flows
☐ Admin Operations
```

---

## 📝 6. สำหรับการเขียน Test Cases

### 6.1 Test Case Categories
```
☐ Functional Test Cases (Happy Path)
☐ Negative Test Cases (Error Scenarios)
☐ Edge Cases (Boundary Values)
☐ Integration Test Cases
☐ UI/UX Test Cases
☐ Accessibility Test Cases
☐ Performance Test Cases
☐ Security Test Cases
```

### 6.2 Test Case Template
```markdown
## Test Case ID: TC-001
**Feature:** User Login
**Priority:** High

### Preconditions
- User account exists in database
- User is on login page

### Test Steps
1. Enter valid username
2. Enter valid password
3. Click "Login" button

### Expected Result
- User is redirected to dashboard
- Session token is created
- Welcome message displayed

### Edge Cases
- Empty fields
- Invalid credentials
- Locked account
- SQL injection attempts
```

### 6.3 Test Data Requirements
```
☐ Valid User Data (different roles)
☐ Invalid Data (for negative testing)
☐ Boundary Values (min/max lengths)
☐ Special Characters / Unicode
☐ Large Data Sets (performance)
☐ Real-world Scenarios
```

---

## 🎯 7. Business Logic & Rules

### 7.1 Business Requirements
```
☐ Business Rules Documentation
☐ Calculation Logic (Formulas)
☐ Validation Rules
☐ Workflow States (State Machine)
☐ Permission Matrix (RBAC/ACL)
☐ Audit Trail Requirements
```

### 7.2 Domain Knowledge
```
☐ Industry Terminology (Glossary)
☐ Business Process Flows
☐ Compliance Requirements (GDPR, etc.)
☐ SLAs (Service Level Agreements)
☐ Error Handling Business Rules
```

---

## 🚀 8. Deployment & Operations

### 8.1 Deployment Information
```
☐ Deployment Process
☐ Environment Configurations
☐ Feature Flags/Toggles
☐ Rollback Procedures
☐ Database Migration Process
```

### 8.2 Monitoring & Logging
```
☐ Log Locations & Formats
☐ Monitoring Dashboards
☐ Alert Thresholds
☐ Error Tracking (Sentry, etc.)
☐ Performance Metrics
```

---

## 📊 9. ข้อมูลเพิ่มเติมที่ควรมี

### 9.1 Historical Data
```
☐ Known Issues / Bug History
☐ Previous Test Results
☐ Performance Benchmarks
☐ User Feedback / Analytics
☐ Incident Reports
```

### 9.2 Contact & Communication
```
☐ Development Team Contacts
☐ Product Owner / Business Analyst
☐ DevOps / Infrastructure Team
☐ Escalation Path
☐ Communication Channels (Slack, etc.)
```

---

## ✅ Quick Checklist Summary

### ขั้นตอนการ Onboard โปรเจกต์ใหม่:

**Week 1: Understand**
- [ ] ได้รับ Access ทั้งหมด (Repo, Environments, Docs)
- [ ] อ่าน PRD และ User Stories
- [ ] ศึกษา Architecture Diagram
- [ ] ทำความเข้าใจ Tech Stack
- [ ] รันโปรเจกต์ใน Local ได้

**Week 2: Explore**
- [ ] ลองใช้งาน Feature หลักทั้งหมด
- [ ] ศึกษา API Documentation
- [ ] ดู Database Schema
- [ ] อ่าน Code ส่วนสำคัญ
- [ ] รู้จัก CI/CD Pipeline

**Week 3: Test Preparation**
- [ ] เขียน Test Plan
- [ ] สร้าง Test Cases สำหรับ Feature หลัก
- [ ] เตรียม Test Data
- [ ] ตั้งค่า Test Environment
- [ ] รัน Existing Tests

**Week 4: Ready**
- [ ] ทดสอบ Feature แรกได้
- [ ] Report Bug แรกได้
- [ ] เข้าใจ Impact Analysis
- [ ] พร้อมทำงานจริง

---

*เอกสารนี้สร้างเมื่อ: 2026-02-26*  
*สร้างโดย: เสี่ยวทู่ 🐰*
