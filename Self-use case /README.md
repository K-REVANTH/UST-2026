# Post-Deployment Validation Framework (Self use case)

## PROBLEM

After a production deployment, the application becomes unstable due to process duplication, port conflicts, network misconfiguration, configuration drift, permission changes, and increasing error logs. An automated system is required to detect these issues, remediate recoverable problems, validate application health, and determine whether the deployment should be marked successful or failed.

---

## APPROACH

### Phase 1: Initial Setup

- Define application name  
- Define expected port  
- Define expected user/group  
- Define configuration file location  
- Define log file location  
- Initialize logging mechanism  
- Define structured exit codes  

---

### Phase 2: Process Check

- Verify application is running  
- Ensure only one instance is active  
- If not running → Fail deployment  

---

### Phase 3: Port Check

- Confirm expected port is bound  
- Ensure correct process owns the port  
- If port conflict detected → Fail deployment  

---

### Phase 4: Configuration Check

- Compare active configuration with baseline  
- If mismatch detected → Restore backup  

---

### Phase 5: Permission Check

- Scan application directory  
- Validate file ownership  
- Correct ownership mismatches  

---

### Phase 6: Log Check

- Count occurrences of:
  - ERROR  
  - TIMEOUT  
  - CONNECTION_REFUSED  
- If errors exceed threshold → Mark unstable  

---

### Phase 7: Health Check

- Call health endpoint using `curl`  
- If HTTP 200 → Success  
- Else → Fail deployment  

---

### Phase 8: Return Status

- Exit 0 → Success  
- Exit 1 → Failure  

---

## WORKFLOW

```
Deployment Completed
        ↓
Run Validation Script
        ↓
System Checks
        ↓
Drift Detection
        ↓
Log Analysis
        ↓
Health Validation
        ↓
Return CI/CD Status
```

