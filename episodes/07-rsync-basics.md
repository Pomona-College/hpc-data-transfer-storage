---
title: "Introduction to rsync"
teaching: 25
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- What is rsync and why is it preferred for large file transfers?
- How do I upload and download files with rsync?
- What do the common rsync flags mean?

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

After completing this episode, participants will be able to:
- Explain rsync's advantages over other transfer methods
- Use rsync to upload files to Sagehen
- Use rsync to download files from Sagehen
- Choose appropriate flags for different transfer scenarios

::::::::::::::::::::::::::::::::::::::::::::::::::

## Why rsync?

**rsync** (remote synchronization) is a command-line tool designed for efficient, reliable file transfer. Key advantages:

1. **Incremental**: Only transfers changed data on subsequent runs
2. **Resumable**: Can resume interrupted transfers with `-P` flag
3. **Efficient**: Compresses data during transfer with `-z` flag
4. **Scriptable**: Ideal for automated and batch operations

rsync comes pre-installed on macOS and Linux. Windows users need WSL or Cygwin.

## Basic Syntax

**Upload to Sagehen:**
```bash
rsync -avhP local_file username@sagehen.hpc.pomona.edu:/rhome/username/destination/
```

**Download from Sagehen:**
```bash
rsync -avhP username@sagehen.hpc.pomona.edu:/rhome/username/source_file local_destination/
```

## Essential Flags

| Flag | Meaning | When to Use |
|------|---------|-------------|
| `-a` | Archive mode (preserves permissions, timestamps) | Always |
| `-v` | Verbose output | Debugging |
| `-h` | Human-readable sizes (MB, GB) | Always |
| `-P` | Show progress + keep partial files for resume | Large files |
| `-r` | Recursive (directory contents) | Directories |
| `-z` | Compress during transfer | Slow networks |

::::::::::::::::::::::::::::::::::::: callout

## Trailing Slashes Matter

```bash
# WITH trailing slash: copies CONTENTS of myproject into /dest/
rsync -avhP ~/myproject/ user@host:/dest/

# WITHOUT trailing slash: copies the DIRECTORY itself, creating /dest/myproject/
rsync -avhP ~/myproject user@host:/dest/
```

This is the most common rsync mistake. Use trailing slashes when you want to transfer directory **contents**.

::::::::::::::::::::::::::::::::::::::::::::::::::

## Uploading a Single File

```bash
rsync -avhP ~/mydata.csv username@sagehen.hpc.pomona.edu:/rhome/username/data/
```

Output:
```
mydata.csv
    50,000,000 100%   15.23MB/s    0:00:03 (xfr#1, to-chk=0/1)

sent 50,001,234 bytes  received 35 bytes  13,333,664 bytes/sec
total size is 50,000,000  speedup is 1.00
```

## Uploading a Directory

```bash
rsync -avhPr ~/myproject/ username@sagehen.hpc.pomona.edu:/rhome/username/projects/myproject/
```

All files and subdirectories are transferred recursively.

## Downloading Files

```bash
rsync -avhP username@sagehen.hpc.pomona.edu:/rhome/username/results/output.h5 ~/Downloads/
```

## Resuming Interrupted Transfers

The `-P` flag is essential. If a transfer is interrupted, simply run the same command again:

```bash
# Original command (interrupted by network drop)
rsync -avhP ~/large_file.tar.gz username@sagehen.hpc.pomona.edu:/rhome/username/

# Resume by running the exact same command
rsync -avhP ~/large_file.tar.gz username@sagehen.hpc.pomona.edu:/rhome/username/
```

rsync detects the existing partial file and resumes from where it stopped.

## Incremental Synchronization

On the second run, rsync only transfers new or changed files:

```bash
# First run: transfers all files
rsync -avhPr ~/myproject/ username@sagehen.hpc.pomona.edu:/rhome/username/myproject/

# Add a new file locally, then run again
rsync -avhPr ~/myproject/ username@sagehen.hpc.pomona.edu:/rhome/username/myproject/
# Only the new file transfers; unchanged files are skipped
```

::::::::::::::::::::::::::::::::::::: challenge

## Challenge: First rsync Transfer

1. Create a 10 MB test file:
   ```bash
   dd if=/dev/zero of=test_data.bin bs=1M count=10
   ```
2. Upload to Sagehen:
   ```bash
   rsync -avhP test_data.bin username@sagehen.hpc.pomona.edu:/rhome/username/
   ```
3. Verify on Sagehen:
   ```bash
   ls -lh /rhome/username/test_data.bin
   ```

What was your transfer speed? Does the file size match?

::::::::::::::::::::::::::::::::::::: solution

Expected output:
```
test_data.bin
    10,485,760 100%    8.34MB/s    0:00:01 (xfr#1, to-chk=0/1)
```

On Sagehen, `ls -lh` should show 10M. The file transferred completely.

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- rsync is the fastest and most reliable method for large file transfers
- Always use `-P` for large files to enable resume capability
- The trailing slash on source paths is critical: `source/` vs `source`
- rsync automatically skips unchanged files on subsequent runs
- Use `-avhP` as your standard flag combination

::::::::::::::::::::::::::::::::::::::::::::::::::
