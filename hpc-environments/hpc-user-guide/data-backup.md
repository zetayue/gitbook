---
description: Be aware that your data can be easily damaged on HPCs
---

# Data Backup

Simply storing data under `/raid` without a backup is unsafe since we use the [RAID 0](https://en.wikipedia.org/wiki/Standard_RAID_levels#RAID_0) configuration on the DGX and DLS! 

It is **mandatory** that all users periodly back up their data by themselves using their preferred ways.

### 1. On DGX

All users are **required** to back up their data under their own back-up directories:

`/data/dgx/backup/user_name`

{% hint style="info" %}
The data under `/data/dgx/backup` is stored in an external 8TB disk. It is also recommended that all users still make another copy of their important data outside the DGX.
{% endhint %}

### 2. On DLS

Since there are no back-up directories on the DLS, all users have to periodly back up their important data outside the DLS. 

