# Post-Deployment Validation Framework (Self use case)

## PROBLEM

After a production deployment, the application becomes unstable due to process duplication, port conflicts, network misconfiguration, configuration drift, permission changes, and increasing error logs. An automated system is required to detect these issues, remediate recoverable problems, validate application health, and determine whether the deployment should be marked successful or failed.

---

## APPROACH

### Phase 1: Initial Setup
Define application name, expected port, expected user/group, configuration file location, log file location, structured exit codes, logging mechanism.

### Phase 2: Process Check
Verify application is running, and ensure only one instance is active. If not running → Fail deployment.

### Phase 3: Port Check
Confirm expected port is bound and ensure correct process owns the port. If port conflict detected → Fail deployment.

### Phase 4: Configuration Check
Compare active configuration with baseline. If mismatch detected → Restore backup.

### Phase 5: Permission Check
Scan application directory, Validate file ownership and Correct ownership mismatches.

### Phase 6: Log Check
Count occurrences of ERROR, TIMEOUT and CONNECTION_REFUSED. If errors exceed threshold → Mark unstable.  

### Phase 7: Health Check
Call health endpoint using `curl`. If HTTP 200 → Success, Else → Fail deployment. 

### Phase 8: Return Status
Exit 0 → Success, Exit 1 → Failure.

