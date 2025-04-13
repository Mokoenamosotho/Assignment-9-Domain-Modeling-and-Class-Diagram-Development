# Class Diagram

 Class diagram for the hospital database management system with backup, recovery, AI, monitoring, and compliance features.

 classDiagram

class User {
  -userId: String
  -name: String
  -email: String
  -role: String
  +login(): Boolean
  +receiveNotification(msg: String): void }

class Database {
  -dbId: String
  -name: String
  -status: String8
  -lastBackupTime: Date
  -location: String
  +getStatus(): String
  +updateStatus(status: String): void}

class Backup {
  -backupId: String
  -timestamp: Date
  -status: String
  -storageLocation: String
  -isEncrypted: Boolean
  +createBackup(): Boolean
  +encryptBackup(): void}

class Recovery {
  -recoveryId: String
  -status: String
  -startTime: Date
  -endTime: Date
  +initiateRecovery(): Boolean
  +cancelRecovery(): void}

class AIModel {
  -modelId: String
  -accuracy: Float
  -lastUpdated: Date
  +predictFailure(db: Database): Boolean
  +detectAnomaly(db: Database): Boolean
  +updateModel(): void}

class AuditLog {
  -logId: String
  -timestamp: Date
  -action: String
  -details: String
  +createLogEntry(): void
  +fetchLogs(): List
}

class ComplianceReport {
  -reportId: String
  -generatedDate: Date
  -contentSummary: String
  +generateReport(): void
  +exportPDF(): File}

%% Relationships

User "1" --> "0..*" Recovery : initiates
User "1" --> "0..*" AuditLog : logsAction
Database "1" --> "0..*" Backup : has
Database "1" --> "0..*" Recovery : usedBy
Recovery "1" --> "1" Backup : uses
AIModel "1" --> "0..*" Database : analyzes
AIModel "1" --> "0..*" AuditLog : logsFindings
AuditLog "1" --> "0..*" ComplianceReport : supports
ComplianceReport "1" --> "0..*" Backup : references
ComplianceReport "1" --> "0..*" Recovery : summarizes


