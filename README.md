# Assignment-9-Domain-Modeling-and-Class-Diagram-Development

# Intelligent Database Backup & Recovery System

## 📌 Project Overview

This project focuses on the design of an intelligent, automated database backup and recovery system using AI for failure prediction, real-time monitoring, and compliance with data regulations like POPIA and HIPAA. The system is designed for hospital or enterprise IT environments.

---

## ✅ User Stories

| **Story ID** | **User Story** | **Acceptance Criteria** | **Priority** |
|--------------|----------------|--------------------------|--------------|
| US-001 | As a DBA, I want the system to create backups at predefined intervals so that data is not lost. | Backups must occur every 24 hours and be logged. | High |
| US-002 | As IT support, I want failure prediction so I can fix issues before downtime. | Alerts must occur 24 hours before failure with 95% accuracy. | High |
| US-003 | As IT support, I want the system to restore from backup automatically to minimize downtime. | RTO: 30 mins, RPO: 1 hour. | High |
| US-004 | As a Security Officer, I want anomaly detection for monitoring threats. | Alerts within 5 mins, logs recorded. | High |
| US-005 | As a DBA, I want manual override of recovery to intervene when necessary. | UI interface available for manual control. | Medium |
| US-006 | As a Security Officer, I want encrypted backup and recovery. | AES-256 encryption used. | High |
| US-007 | As a Hospital Manager, I want real-time database health metrics. | Dashboard shows live metrics. | Medium |
| US-008 | As a Compliance Auditor, I want audit logs of backup/recovery. | Logs must be stored for 12 months. | High |
| US-009 | As a Compliance Auditor, I want compliance reports generated. | Reports include consent, retention, and logs. | High |
| US-010 | As an AI System, I want to retrain models to improve predictions. | Models updated every 30 days with 95% accuracy. | Medium |

---

## 🧱 Domain Model

| Entity             | Attributes                                                                 | Methods                                                       | Relationships                                         |
|--------------------|---------------------------------------------------------------------------|---------------------------------------------------------------|------------------------------------------------------|
| Database           | id, name, size, status, lastBackupDate                                     | backup(), restore(), updateStatus()                           | Associated with Backup and Recovery                 |
| Backup             | id, timestamp, location, encryptionUsed, status                            | createBackup(), verifyIntegrity()                             | Linked to Database (1..1)                           |
| Recovery           | id, recoveryTime, recoveryStatus                                           | initiateRecovery(), rollback(), pauseRecovery()               | Associated with Backup and Database (1..1)          |
| AIModel            | id, modelType, accuracy, lastTrained                                       | predictFailure(), trainModel()                                | Monitors Database (1..*)                            |
| AuditLog           | id, timestamp, user, action                                                | logEvent(), generateReport()                                  | Logs actions from Backup and Recovery               |
| User               | id, name, role, contactInfo                                                | login(), triggerBackup(), approveRecovery()                   | Can act on Backup and Recovery                      |
| ComplianceReport   | id, reportDate, policyName, status                                         | generateComplianceReport()                                    | Uses AuditLog (1..*)                                |

### Business Rules

- A backup must be created at least every 24 hours.
- Only authorized users can trigger recovery.
- Audit logs must be retained for 12 months.
- AI model must be retrained every 30 days.
- Data backups must be encrypted using AES-256.
- RTO (Recovery Time Objective) must not exceed 30 minutes.
- RPO (Recovery Point Objective) must not exceed 1 hour.

---

## 🧬 Class Diagram (Mermaid.js)

```mermaid
classDiagram
class Database {
  -id: String
  -name: String
  -size: Number
  -status: String
  -lastBackupDate: Date
  +backup()
  +restore()
  +updateStatus()
}

class Backup {
  -id: String
  -timestamp: Date
  -location: String
  -encryptionUsed: Boolean
  -status: String
  +createBackup()
  +verifyIntegrity()
}

class Recovery {
  -id: String
  -recoveryTime: Date
  -recoveryStatus: String
  +initiateRecovery()
  +rollback()
  +pauseRecovery()
}

class AIModel {
  -id: String
  -modelType: String
  -accuracy: Float
  -lastTrained: Date
  +predictFailure()
  +trainModel()
}

class AuditLog {
  -id: String
  -timestamp: Date
  -user: String
  -action: String
  +logEvent()
  +generateReport()
}

class User {
  -id: String
  -name: String
  -role: String
  -contactInfo: String
  +login()
  +triggerBackup()
  +approveRecovery()
}

class ComplianceReport {
  -id: String
  -reportDate: Date
  -policyName: String
  -status: String
  +generateComplianceReport()
}

Database "1" -- "1" Backup : has
Database "1" -- "1" Recovery : restoresFrom
AIModel "1" -- "*" Database : monitors
Backup "1" -- "*" AuditLog : logs
Recovery "1" -- "*" AuditLog : logs
User "1" -- "*" Backup : manages
User "1" -- "*" Recovery : manages
ComplianceReport "1" -- "*" AuditLog : uses


🛠 Technologies Used
Mermaid.js for class diagrams
Markdown for documentation
Object-Oriented Design principles
AI & Encryption concepts (theoretical)


---


