---
description: Be aware that your data can be easily damaged on HPCs
---

# Data Backup

Simply storing data under `/raid` without a backup is unsafe since we use the [RAID 0](https://en.wikipedia.org/wiki/Standard\_RAID\_levels#RAID\_0) configuration on the DGX and DLS!

It is **mandatory** that all users periodically back up their data (1) under the HPCs' own backup directories and (2) by themselves using their preferred ways.

### 1. Backup Directories

<table><thead><tr><th width="172">HPC</th><th>Backup Directory</th></tr></thead><tbody><tr><td>DGX</td><td><code>/data/dgx/backup</code></td></tr><tr><td>DLS</td><td><code>/data/dls1/backup</code></td></tr></tbody></table>

### 2. Check Disk Space

Before having the backup on HPCs' own directories, be sure to check about the remaining disk space via command

```
df -lh
```

The disk usage information can be found related to the backup directory.
