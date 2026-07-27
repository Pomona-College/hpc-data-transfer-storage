---
title: 'Reference'
---

## Storage Locations

### Persistent Storage

| Location | Purpose | Quota | Backed Up | Notes |
|----------|---------|-------|-----------|-------|
| `/rhome/username` | Home directory | 100 GB | Yes (weekly) | Personal, not shareable within lab |
| `/bigdata/labname` | Lab shared storage | 1 TB per lab | Yes (weekly) | Shared with all lab members |

### Temporary Storage

| Location | Purpose | Quota | Lifetime | Speed |
|----------|---------|-------|----------|-------|
| `/scratch/$SLURM_JOB_ID` | Job-local SSD | Unlimited | Duration of job | Very fast (SSD-backed) |
| `/tmpfs/$SLURM_JOB_ID` | RAM-backed temp | Limited | Duration of job | Fastest (in-memory) |

## Checking Quotas

### Check your home directory quota:

```bash
quota -s
```

Output example:
```
Filesystem                used   quota   limit   grace
/rhome/afrancis           2.4G   25G     27.5G
/bigdata/neuroscience     145G   1.0T    1.1T
```

### Check individual directory sizes:

```bash
du -sh /rhome/afrancis/
du -sh /bigdata/neuroscience/project_alpha/
```

### Monitor real-time storage usage during jobs:

```bash
# In your SLURM job
df -h /scratch/$SLURM_JOB_ID /tmpfs/$SLURM_JOB_ID
```

## File Transfer Commands

### SCP (Secure Copy) - Simple Point-to-Point

**Download from Sagehen to your computer:**

```bash
scp afrancis@sagehen.hpc.pomona.edu:/rhome/afrancis/results.tar.gz ./
```

**Upload to Sagehen from your computer:**

```bash
scp large_dataset.tar.gz afrancis@sagehen.hpc.pomona.edu:/bigdata/neuroscience/
```

**Copy entire directories (recursive):**

```bash
scp -r afrancis@sagehen.hpc.pomona.edu:/rhome/afrancis/project/ ./local-project/
```

### Rsync - Efficient Incremental Sync

Rsync only transfers changed files and is ideal for large datasets.

**Basic sync (one-way, local to Sagehen):**

```bash
rsync -av --progress local_folder/ afrancis@sagehen.hpc.pomona.edu:/bigdata/neuroscience/
```

**Sync with deletion (mirror):**

```bash
rsync -av --delete local_folder/ afrancis@sagehen.hpc.pomona.edu:/bigdata/neuroscience/
```

**Sync from Sagehen to local (download):**

```bash
rsync -av --progress afrancis@sagehen.hpc.pomona.edu:/bigdata/neuroscience/results/ ./local_results/
```

**Common flags:**
- `-a` : Archive mode (preserves permissions, timestamps, symlinks)
- `-v` : Verbose output
- `--progress` : Show progress during transfer
- `--delete` : Delete files on destination that don't exist on source
- `--exclude='*.tmp'` : Exclude file patterns
- `-e 'ssh -p 22'` : Specify SSH port (if non-standard)

### SFTP (Interactive File Transfer)

**Using FileZilla (GUI):**
1. Host: `sagehen.hpc.pomona.edu`
2. Protocol: `SFTP`
3. Port: `22`
4. Username: Your Pomona AD username
5. Password: Your Pomona AD password (or SSH key)
6. Click "Quickconnect"

**Using command-line SFTP:**

```bash
sftp afrancis@sagehen.hpc.pomona.edu
```

Common SFTP commands:
```
ls              # List remote files
pwd             # Print remote working directory
cd /rhome/afrancis   # Change remote directory
get filename    # Download file
put filename    # Upload file
mget *.txt      # Download multiple files
mput *.txt      # Upload multiple files
exit            # Close connection
```

### Rclone - Cloud & Remote Sync

Rclone can sync files between Sagehen, cloud storage (Google Drive, Dropbox, AWS S3, etc.), and your local machine.

**Configure Sagehen as a remote (one-time setup):**

```bash
rclone config
```

Select:
- Name: `sagehen`
- Type: `sftp`
- Host: `sagehen.hpc.pomona.edu`
- User: Your username
- Port: `22`
- Ask for password: `false` (if using SSH keys)

**Sync from Google Drive to Sagehen:**

```bash
rclone sync gdrive:/Research/Project1 sagehen:/bigdata/labname/Project1 --progress
```

**Sync from Sagehen to AWS S3 (backup):**

```bash
rclone sync sagehen:/bigdata/labname/data s3://my-research-bucket/backup --progress
```

**Dry-run to see what would be transferred:**

```bash
rclone sync --dry-run gdrive:/Dataset sagehen:/bigdata/labname/Dataset
```

## Data Archiving

### Create compressed archives:

**tar + gzip (good compression, slower):**

```bash
tar -czf archive.tar.gz /rhome/afrancis/project/
```

**tar + bzip2 (better compression, much slower):**

```bash
tar -cjf archive.tar.bz2 /rhome/afrancis/project/
```

**tar + xz (best compression, slowest):**

```bash
tar -cJf archive.tar.xz /rhome/afrancis/project/
```

### Extract archives:

```bash
tar -xzf archive.tar.gz
tar -xjf archive.tar.bz2
tar -xJf archive.tar.xz
```

## Monitoring Transfers

### Watch transfer progress with rsync:

```bash
rsync -av --progress afrancis@sagehen.hpc.pomona.edu:/bigdata/neuroscience/large_file.bin ./
```

### Monitor bandwidth usage on Sagehen:

```bash
# SSH into Sagehen first
ifstat -i eth0 1  # Update every 1 second
```

## Storage Best Practices Quick Tips

1. **Use `/scratch/` for large intermediate files** during jobs: they're fast and automatically cleaned up
2. **Move results to `/bigdata/` or `/rhome/`** before the job ends to preserve them
3. **Archive old projects** to compress them and reduce quota usage
4. **Use rsync for large transfers**: it's resumable if interrupted
5. **Check quotas regularly** with `quota -s` to avoid running out of space
6. **Back up important data locally**: Sagehen has weekly snapshots but aren't a substitute for backups

## Contact Information

- **HPC Support:** its-hpc@pomona.edu
- **OnDemand:** https://ondemand.hpc.pomona.edu/
- **Cluster Status:** Check email or contact support for maintenance windows
- **Emergency Issues:** its-hpc@pomona.edu (include error messages and job IDs)
