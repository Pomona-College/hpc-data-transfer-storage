---
title: "Checking and Managing Quotas"
teaching: 20
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- How much storage can I use on Sagehen?
- How do I check my current storage usage?
- Why does `du` give inaccurate results on BeeGFS?

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

After completing this episode, participants will be able to:
- Use `quota_check.sh` to monitor storage usage
- Understand the difference between apparent size and block size on BeeGFS
- Use `du` with the correct flags for accurate measurements
- Identify large files and directories consuming space

::::::::::::::::::::::::::::::::::::::::::::::::::

## Quota Structure on Sagehen

Both `/rhome/username` and `/bigdata/labname` share a **lab-based quota**:

- **Quota per lab**: 1TB combined across home + lab storage
- **Shared responsibility**: All lab members share this quota
- **Enforcement**: Hard limit -- cannot write new files if exceeded
- **Exceptions**: Request temporary increase by contacting its-hpc@pomona.edu

```
Lab ABC (quota: 1TB = 1024GB)
+-- /rhome/alice       (150 GB)
+-- /rhome/bob         (120 GB)
+-- /rhome/charlie     (180 GB)
+-- /bigdata/abc       (574 GB)
    Total Used: 1024 GB (at limit!)
```

Temporary storage (`/scratch` and `/tmpfs`) does not count against your lab quota.

## Using quota_check.sh (Recommended)

Sagehen provides a dedicated script that accounts for BeeGFS:

```bash
quota_check.sh
```

**Example output:**
```
Quota check for user: alice

Home Directory: /rhome/alice
  Used:     150.2 GB
  Quota:    1024 GB
  Percent:  14.6%

Lab Storage: /bigdata/labname
  Used:     574.3 GB
  Quota:    1024 GB
  Percent:  56.1%

Combined Usage:
  Total Used:   724.5 GB
  Total Quota:  1024 GB
  Percent:      70.7%

Status: OK (below quota)
```

Status levels: **OK** (below 80%), **WARNING** (above 80%), **CRITICAL** (above 95%).

## Using du Correctly on BeeGFS

The `du` command requires `--apparent-size` for accuracy on BeeGFS:

```bash
# Inaccurate (reports block usage)
du -sh /rhome/username
  Result: 156 GB

# Accurate (reports actual data size)
du -sh --apparent-size /rhome/username
  Result: 150 GB
```

**Always use** `du --apparent-size` on Sagehen.

## Finding Large Files and Directories

**Largest directories:**
```bash
du --apparent-size -h /rhome/username | sort -h
```

**Largest individual files:**
```bash
find /rhome/username -type f -printf '%s %p\n' | sort -rn | head -20
```

## What Happens When You Exceed Quota

At 100% quota, you **cannot write new files**:

```bash
$ cp large_file.dat /rhome/username/
cp: cannot create regular file: Disk quota exceeded
```

You can still read files, list directories, and run jobs that only read from disk. But you cannot write, modify, or save job outputs.

To request a quota increase, contact its-hpc@pomona.edu with your lab name, justification, specific data size needed, and cleanup timeline.

::::::::::::::::::::::::::::::::::::: challenge

## Challenge: Check Your Quota

1. Run `quota_check.sh` and record your current usage
2. Find your 5 largest directories:
   ```bash
   du --apparent-size -h /rhome/username | sort -h | tail -5
   ```
3. Compare your home directory usage with lab storage

Are you above or below 80% usage? Which is larger: your home directory or lab storage?

::::::::::::::::::::::::::::::::::::: solution

Sample output:
```bash
$ quota_check.sh
Combined Usage:
  Total Used:   724.5 GB
  Total Quota:  1024 GB
  Percent:      70.7%
Status: OK (below quota)
```

Your specific numbers will vary. If above 80%, consider cleanup strategies covered in the next episode.

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Use `quota_check.sh` for official quota monitoring on Sagehen
- Always use `du --apparent-size` on BeeGFS for accurate size measurements
- Your lab shares a 1TB quota across all members and both `/rhome` and `/bigdata`
- You cannot write files if quota is exceeded -- monitor regularly
- Temporary storage (`/scratch`, `/tmpfs`) does not count against quota

::::::::::::::::::::::::::::::::::::::::::::::::::
