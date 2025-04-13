# 🧩 Domain Model

## 📘 Domain Entities

### 1. User

| Attribute         | Type                                                               | Description                                  |
|------------------|--------------------------------------------------------------------|----------------------------------------------|
| `userId`         | String                                                             | Unique identifier for the user               |
| `name`           | String                                                             | Full name of the user                        |
| `email`          | String                                                             | Email address                                |
| `phoneNumber`    | String                                                             | Contact number for alerts                    |
| `role`           | Enum (Admin, Auditor, SecurityOfficer, ITSupport, HospitalManager) | Defines user’s access level                  |
| `preferredLanguage` | String                                                         | Language for UI and notifications            |

**Responsibilities:**
- Authenticate and log in  
- Receive alerts and notifications  
- Access dashboards and logs based on role  

---

### 2. Database

| Attribute        | Type                              | Description                                  |
|-----------------|-----------------------------------|----------------------------------------------|
| `dbId`          | String                            | Unique ID for the database                   |
| `name`          | String                            | Name of the database                         |
| `status`        | Enum (Healthy, Failed, Recovering)| Current health status                        |
| `lastBackupTime`| DateTime                          | Timestamp of the last backup                 |
| `location`      | String                            | Physical or cloud location                   |
| `dataSize`      | Number (GB)                       | Size of the data stored                      |

**Responsibilities:**
- Maintain current status and metrics  
- Link to backup and recovery operations  

---

### 3. Backup

| Attribute        | Type                                  | Description                                  |
|-----------------|---------------------------------------|----------------------------------------------|
| `backupId`      | String                                | Unique backup identifier                     |
| `dbId`          | String                                | ID of the database being backed up           |
| `timestamp`     | DateTime                              | When the backup was taken                    |
| `status`        | Enum (Successful, Failed, InProgress) | Current status of backup                     |
| `storageLocation` | String                              | Where the backup is stored                   |
| `isEncrypted`   | Boolean                               | Whether the backup is encrypted              |
| `encryptionKeyId`| String                               | Key used for encryption, if any              |

**Responsibilities:**
- Create encrypted snapshots of the database  
- Store logs and status of backup tasks  

---

### 4. Recovery

| Attribute        | Type                                              | Description                                  |
|-----------------|---------------------------------------------------|----------------------------------------------|
| `recoveryId`    | String                                            | Unique identifier for the recovery operation |
| `dbId`          | String                                            | Associated database                          |
| `usedBackupId`  | String                                            | Backup used for recovery                     |
| `initiatedBy`   | String                                            | User ID or system trigger                    |
| `status`        | Enum (InProgress, Successful, Failed, Cancelled)  | Recovery state                               |
| `startTime`     | DateTime                                          | When recovery began                          |
| `endTime`       | DateTime                                          | When recovery completed                      |

**Responsibilities:**
- Restore database from valid backup  
- Log status and allow manual override (pause/resume/cancel)  

---

### 5. AIModel

| Attribute            | Type                              | Description                                  |
|---------------------|-----------------------------------|----------------------------------------------|
| `modelId`           | String                            | Unique AI model identifier                   |
| `trainingDataVersion`| String                           | Version of dataset used for training         |
| `lastUpdated`       | DateTime                          | When model was last trained                  |
| `accuracyRate`      | Float                             | Accuracy of predictions                      |
| `status`            | Enum (Active, Retraining)         | State of AI model                            |

**Responsibilities:**
- Predict database failures based on monitoring  
- Detect anomalies and generate alerts  

---

### 6. AuditLog

| Attribute        | Type       | Description                                  |
|-----------------|------------|----------------------------------------------|
| `logId`         | String     | Unique log identifier                        |
| `userId`        | String     | User performing the action                   |
| `timestamp`     | DateTime   | Time of action                               |
| `action`        | String     | Description of action taken                  |
| `details`       | String     | Extended log details if needed               |

**Responsibilities:**
- Track all system and user actions  
- Provide traceability for compliance and reporting  

---

## 🔗 Entity Relationships

| Entity     | Related To     | Relationship Description                                         |
|------------|----------------|------------------------------------------------------------------|
| `User`     | `Recovery`     | A user can initiate or override a recovery                      |
| `User`     | `AuditLog`     | A user generates log entries through system actions             |
| `Database` | `Backup`       | A database can have many backups                                |
| `Database` | `Recovery`     | A database may be restored through one or more recoveries       |
| `AIModel`  | `Database`     | Monitors and analyzes behavior of one or more databases         |
| `AIModel`  | `AuditLog`     | Logs AI predictions and anomaly detections                      |

---

## 📏 Business Rules

- **Automated Backups:** A database must be backed up at least once every 24 hours.  
- **Manual Overrides:** Only users with the correct role (e.g., Admin, ITSupport) can override automated recovery.  
- **Encryption:** All backups must use AES-256 encryption if `isEncrypted` is true.  
- **AI Training:** AI models must be retrained every 30 days to maintain prediction accuracy above 95%.  
- **Audit Retention:** Audit logs must be retained and accessible for at least 12 months.  
- **RTO and RPO:** Recovery must meet an RTO of 30 minutes and RPO of 1 hour.  



| Entity            | Attributes                                                                 | Methods                                                                 | Relationships                                           |
|------------------|------------------------------------------------------------------------------|-------------------------------------------------------------------------|---------------------------------------------------------|
| User             | userId, name, email, phoneNumber, role, preferredLanguage                   | login(), receiveNotification()                                         | Initiates Recovery, Triggers Alerts, Logs actions in AuditLog |
| Database         | dbId, name, status, lastBackupTime, location, dataSize                      | getStatus(), updateStatus(), attachBackup()                            | Has many Backups and Recoveries, Monitored by AIModel   |
| Backup           | backupId, dbId, timestamp, status, storageLocation, isEncrypted, encryptionKeyId | createBackup(), encryptBackup()                                        | Belongs to Database, Used by Recovery                   |
| Recovery         | recoveryId, dbId, usedBackupId, initiatedBy, status, startTime, endTime     | initiateRecovery(), cancelRecovery(), getRecoveryTime()                | Uses Backup, Initiated by User                          |
| AIModel          | modelId, trainingDataVersion, lastUpdated, accuracyRate, status             | predictFailure(), detectAnomaly(), updateModel()                       | Analyzes Database, Logs into AuditLog                   |
| AuditLog         | logId, userId, timestamp, action, details                                    | createLogEntry(), fetchLogs()                                          | Logs actions from User and AIModel                      |
| ComplianceReport | reportId, generatedDate, generatedBy, contentSummary, isPOPIACompliant      | generateReport(), exportPDF()                                          | Based on Backup, Recovery, and AuditLog records         |

Business Rules

1. Backups must occur at least once every 24 hours.
2. All backups and recoveries must use AES-256 encryption.
3. Recovery must complete within 30 minutes of failure (RTO), with max 1 hour data loss (RPO).
4. Only Admins or Security Officers can initiate or pause manual recovery processes.
5. Audit logs must be retained for a minimum of 12 months and be accessible for compliance.
6. AI model alerts must meet a 95% prediction confidence threshold before triggering alerts.
7. AI models must be updated at least once every 30 days with the latest operational data.
8. Compliance reports must include audit logs, backup history, and anomaly detection records.
