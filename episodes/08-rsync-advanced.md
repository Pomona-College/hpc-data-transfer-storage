---
title: "Advanced rsync Workflows"
teaching: 20
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- How do I exclude certain files from rsync transfers?
- How do I safely delete remote files not present locally?
- How can I use rsync in SLURM job scripts?

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

After completing this episode, participants will be able to:
- Use exclude patterns to skip unwanted files
- Preview operations with dry-run before executing
- Safely use the --delete flag with proper safeguards
- Integrate rsync into SLURM job scripts

::::::::::::::::::::::::::::::::::::::::::::::::::

## Exclude Patterns

Skip files you do not want to transfer:

```bash
rsync -avhPr --exclude='.git' --exclude='*.pyc' --exclude='__pycache__' \
  ~/myproject/ username@sagehen.hpc.pomona.edu:/rhome/username/myproject/
```

Common exclusions:
```bash
--exclude='*.pyc'        # Python compiled files
--exclude='__pycache__'  # Python cache directories
--exclude='.git'         # Git history
--exclude='.DS_Store'    # macOS metadata files
--exclude='*.tmp'        # Temporary files
--exclude='.venv'        # Virtual environments
```

## Include Only Specific Files

Transfer only matching file types by combining include and exclude:

```bash
rsync -avhPr --include='*.txt' --exclude='*' \
  ~/myproject/ username@sagehen.hpc.pomona.edu:/rhome/username/myproject/
```

## Dry Run (Preview)

See what rsync would do without actually transferring anything:

```bash
rsync -avhPr --dry-run ~/myproject/ username@sagehen.hpc.pomona.edu:/rhome/username/myproject/
```

Output includes `(DRY RUN)` and shows files that would transfer. Always use this before any destructive operation.

## Deleting Remote Files

The `--delete` flag removes files from the destination that do not exist in the source:

```bash
# DANGEROUS: Always dry-run first!
rsync -avhPr --delete --dry-run ~/myproject/ username@sagehen.hpc.pomona.edu:/rhome/username/myproject/

# If the dry-run output looks correct, run without --dry-run
rsync -avhPr --delete ~/myproject/ username@sagehen.hpc.pomona.edu:/rhome/username/myproject/
```

::::::::::::::::::::::::::::::::::::: callout

## Warning: --delete Can Destroy Data

Running `--delete` with an empty source directory will delete everything on the remote side. Always preview with `--dry-run` first.

::::::::::::::::::::::::::::::::::::::::::::::::::

## Compression

For slow connections, compress data during transfer:

```bash
rsync -avhPrz ~/myproject/ username@sagehen.hpc.pomona.edu:/rhome/username/myproject/
```

Compression helps for text-heavy files but provides no benefit for already-compressed formats (images, videos, archives).

## Bandwidth Limiting

Avoid saturating your network by limiting transfer speed:

```bash
rsync -avhPr --bwlimit=10m ~/myproject/ username@sagehen.hpc.pomona.edu:/rhome/username/myproject/
```

Units: `k` = kilobytes/s, `m` = megabytes/s.

## Rsync in Job Scripts

Use rsync to stage data into fast temporary storage during SLURM jobs:

```bash
#!/bin/bash
#SBATCH --job-name=data_process
#SBATCH --time=04:00:00

# Copy input to fast scratch
rsync -avhP /rhome/username/input.dat /scratch/$SLURM_JOB_USER/$SLURM_JOB_ID/

# Run computation on scratch
cd /scratch/$SLURM_JOB_USER/$SLURM_JOB_ID/
./myanalysis input.dat

# Copy results back to permanent storage
rsync -avhP output.dat /rhome/username/results/
```

## Common Mistakes

1. **Wrong trailing slash**: `rsync ~/myproject host:/dest/` creates `/dest/myproject/`, while `rsync ~/myproject/ host:/dest/` puts contents directly in `/dest/`
2. **Forgetting -P**: Without `-P`, large interrupted transfers cannot resume
3. **Using --delete carelessly**: Always dry-run first
4. **Forgetting -r for directories**: Without `-r`, subdirectories are not transferred (though `-a` implies `-r`)

::::::::::::::::::::::::::::::::::::: challenge

## Challenge: Selective Sync with Dry Run

1. Create a local project with mixed files:
   ```bash
   mkdir -p ~/testproject/data ~/testproject/scripts
   echo "data" > ~/testproject/data/data.txt
   echo "print('hello')" > ~/testproject/scripts/main.py
   echo "cache" > ~/testproject/scripts/main.pyc
   ```
2. Dry-run an rsync excluding `.pyc` files:
   ```bash
   rsync -avhPr --exclude='*.pyc' --dry-run ~/testproject/ username@sagehen.hpc.pomona.edu:/rhome/username/testproject/
   ```
3. Verify that `main.pyc` is not listed in the dry-run output
4. Run without `--dry-run` to actually transfer

::::::::::::::::::::::::::::::::::::: solution

The dry-run should show `data/data.txt` and `scripts/main.py` but not `scripts/main.pyc`. The `(DRY RUN)` tag confirms no files were actually transferred. Running without `--dry-run` completes the transfer of only the non-excluded files.

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Use `--exclude` patterns to skip unwanted files like `.git`, `.pyc`, and caches
- Always use `--dry-run` before `--delete` to prevent accidental data loss
- Compression (`-z`) helps on slow networks but not for already-compressed files
- rsync integrates well into SLURM job scripts for staging data to fast storage

::::::::::::::::::::::::::::::::::::::::::::::::::
