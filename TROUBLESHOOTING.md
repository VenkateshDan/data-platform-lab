# Troubleshooting Notes

### 1. HDFS Permission Denied
- **Error**: `AccessControlException: Permission denied: user=airflow`
- **Cause**: Attempted to write to `/` root in HDFS.
- **Fix**: Created `/user/airflow/` and updated output path:
