Here’s a copy-paste ready format you can drop into your **`TROUBLESHOOTING.md`** file 👇

```md
# Troubleshooting Notes

---

### 1. HDFS Permission Denied

- **Error**:  
```

org.apache.hadoop.security.AccessControlException: Permission denied: user=airflow, access=WRITE, inode="/"\:root\:supergroup\:drwxr-xr-x

````

- **Cause**:  
Spark (running under the `airflow` user) attempted to write directly to the root directory `/` in HDFS.  
By default, the HDFS root is owned by `hdfs:supergroup` and normal users cannot write there.

- **Fix**:  
1. Created a user-specific directory in HDFS:  
   ```bash
   hdfs dfs -mkdir -p /user/airflow
   hdfs dfs -chown airflow:supergroup /user/airflow
   ```
2. Updated the Spark job’s output path to:  
   ```
   hdfs://hdfs-namenode:9000/user/airflow/enriched_orders
   ```

- **Takeaway**:  
Always write Spark outputs to user-owned HDFS paths (`/user/<username>/...`) instead of root `/` to avoid permission issues.
````
