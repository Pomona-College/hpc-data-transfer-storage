---
title: "Quota Cleanup Strategies"
teaching: 15
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- How do I free up space when approaching my quota?
- What are effective strategies for reducing storage usage?
- How do I troubleshoot common quota issues?

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

After completing this episode, participants will be able to:
- Implement regular cleanup routines
- Use compression to reduce storage usage
- Identify and remove redundant files
- Troubleshoot common quota problems

::::::::::::::::::::::::::::::::::::::::::::::::::

## Regular Cleanup

Run a monthly maintenance routine:

```bash
# Check quota
quota_check.sh

# Find old files (not accessed in 90 days)
find /rhome/username -type f -atime +90 -exec ls -lh {} \;

# Archive and remove old project
tar czf /bigdata/labname/archive_2025_03.tar.gz /rhome/username/old_project/
rm -rf /rhome/username/old_project/
```

## Compression

Compress old data to reclaim space:

```bash
tar czf project_archive.tar.gz project_name/

# Check compression ratio
du --apparent-size -h project_archive.tar.gz
du --apparent-size -h project_name/
```

Typical compression for text/CSV data: 5-20x smaller.

## Identifying Redundancy

Remove backup and temporary files:

```bash
# Find backup files
find /rhome/username -name "*.bak" -o -name "*_old" -o -name "*~"

# Remove them
find /rhome/username -name "*.bak" -delete
```

Avoid storing multiple manual copies of the same project. Use git version control instead:

```bash
cd /rhome/username/project
git init
git add .
git commit -m "Initial version"
```

## Monitoring and Alerts

Your lab receives email notifications at 80%, 95%, and 100% quota usage. You can also monitor quota during file transfers:

```bash
watch -n 5 quota_check.sh
```

::::::::::::::::::::::::::::::::::::: callout

### Quota Management Checklist

- **Monthly**: Run `quota_check.sh` and review usage
- **Quarterly**: Clean up files older than 6 months
- **After projects**: Archive and remove old project directories
- **Before large jobs**: Verify available quota
- **For temporary work**: Use `/scratch` (does not count against quota)

::::::::::::::::::::::::::::::::::::::::::::::::::

## Troubleshooting Common Issues

### "Disk quota exceeded" but du shows space

Your lab's combined quota may be full even if your home looks fine. Run `quota_check.sh` to see the combined usage. Also check hidden directories:

```bash
du --apparent-size -h /rhome/username/.cache
du --apparent-size -h /rhome/username/.local
```

### du reports different size than quota_check.sh

You likely forgot the `--apparent-size` flag. Without it, `du` reports block usage on BeeGFS, which differs from actual data size.

### Deleted files but usage unchanged

A running process may still hold the file handle open:

```bash
lsof /rhome/username/largefile.dat
kill <process_id>
```

::::::::::::::::::::::::::::::::::::: challenge

## Challenge: Calculate Storage Efficiency

1. Pick a directory and measure its size:
   ```bash
   du --apparent-size -sh /rhome/username/project1
   ```
2. Calculate what portion of your 1TB quota it uses
3. Identify at least one file or directory you could safely delete or compress

::::::::::::::::::::::::::::::::::::: solution

Example:
```
Project directory: 10 GB
Current quota: 1024 GB
Percentage used: 10/1024 = 0.98%
```

Actions to consider: delete `.bak` files, compress older project directories, move shared data to `/bigdata`, remove duplicate datasets.

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Regular cleanup prevents quota-related disruptions
- Compression can reduce text-based data by 5-20x
- Use git instead of manual versioning to avoid duplicate copies
- Always use `du --apparent-size` to get accurate sizes on BeeGFS
- Hidden directories like `.cache` and `.local` can consume significant space

::::::::::::::::::::::::::::::::::::::::::::::::::
