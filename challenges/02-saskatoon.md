# Saskatoon: Counting IPs 🔍

**Type:** Do  
**Time to Solve:** ~15 minutes  

## Challenge Summary

There’s a web server access log file located at `/home/admin/access.log`. Each line represents one HTTP request, with the requester’s IP address appearing at the start of the line.  

- The goal is to determine **which IP address made the most requests**.  
- Then, write the result into `/home/admin/highestip.txt`.  

## Solution Overview

### 1. First, we need to extract the IP addresses from the log file.

- Since the IP address is the first field on each line, we can use `awk` or `cut` to isolate it:

  ```bash
  awk '{print $1}' /home/admin/access.log
  ```

This prints only the first column, which corresponds to the IP addresses.  

### 2. Next, we count how many times each IP occurs.

- To do this, we can sort the list of IPs and then use `uniq -c` to count occurrences:

  ```bash
  awk '{print $1}' /home/admin/access.log | sort | uniq -c
  ```

The output shows each unique IP alongside the number of requests it made.  

### 3. Now, we identify the IP with the highest request count.

- Sort the counted list in descending order by number of requests and take the top result:

  ```bash
  awk '{print $1}' /home/admin/access.log | sort | uniq -c | sort -nr | head -1
  ```

This outputs the most frequent IP along with its request count.  

### 4. Finally, we write just the IP address to the required file.

- Since we only need the IP (not the count), we add `awk '{print $2}'` to extract it:

```bash
awk '{print $1}' /home/admin/access.log | sort | uniq -c | sort -nr | head -1 | awk '{print $2}' > /home/admin/highestip.txt
```

This ensures that `/home/admin/highestip.txt` contains only the IP address with the most requests.  

## Notes

- No root access is required for this challenge since we’re only working with a log file in the home directory.  
- The checksum provided in the instructions can be used to verify that the correct IP was written to the file:
    - "*Test: The SHA1 checksum of the IP address `sha1sum /home/admin/highestip.txt` is `6ef426c40652babc0d081d438b9f353709008e93`*"
