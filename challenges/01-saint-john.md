# Saint John: What is Writing to This Log File? 🤔

**Type:** Fix  
**Time to Solve:** ~10 minutes  

## Challenge Summary

A developer created a testing program that continuously writes to the log file `/var/log/bad.log`. This causes the log to grow rapidly, potentially filling up disk space. 

- The goal is to identify the process responsible for writing to this file and terminate it. 

- Importantly, the log file itself should **not** be deleted.  

## Solution Overview

### 1. To tackle this, we first need to determine which process is writing to the log file.

- Since the log file is constantly being updated, we can watch the latest logs in real time with:

  ```bash
  tail -f /var/log/bad.log
  ```

This lets us see entries as they are added, which sometimes gives clues about the program generating them.  

## 2. The next step is to find the process that has the file open.

- We can do this using:

  ```bash
  lsof /var/log/bad.log
  ```

`lsof` lists all processes currently using a file. 

In the output, look for the **PID** (process ID) of the process writing to `bad.log`.  

## 3. Once the PID is identified, we can terminate this process.

- Use the following command:
  
  ```bash
  kill -9 <PID>
  ```
`kill -9 <PID>` sends the SIGKILL signal, which forcefully terminates the process immediately and cannot be ignored.

After this, monitor the file again with `tail -f` or check its size to ensure it is no longer increasing.  

## Notes

- Root (sudo) access is required to see and terminate the process.  
- The log file itself is **not deleted**; only the offending process is stopped.  
- This method is safe for identifying runaway processes that fill disk space.  

After completing these steps, the solution check script `/home/admin/agent/check.sh` confirms that the log file size no longer changes over time.  
