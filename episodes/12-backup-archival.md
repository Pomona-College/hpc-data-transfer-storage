---
title: "Backup and Archival Strategies"
teaching: 20
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- How do I back up my data on Sagehen?
- When and how should I archive completed projects?
- How do I migrate data between storage locations?

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

After completing this episode, participants will be able to:
- Implement a backup strategy using rsync
- Archive completed projects to free quota space
- Migrate data safely between storage locations
- Plan data retention across the research lifecycle

::::::::::::::::::::::::::::::::::::::::::::::::::

## Backup vs. Archive

**Backup**: Copies for disaster recovery. Sagehen has weekly snapshots -- contact its-hpc@pomona.edu for recovery. You should also maintain local backups.

**Archive**: Long-term storage of completed work. Compressed and moved to free active storage space.

## Weekly Rsync Backup

Back up your Sagehen data to your local machine:

```bash
#!/bin/bash
# sagehen_backup.sh

BACKUP_DIR=$HOME/backups/sagehen_$(date +%Y%m%d)
mkdir -p $BACKUP_DIR

rsync -avhPr \
  --exclude='*.pyc' --exclude='__pycache__' --exclude='.git' \
  username@sagehen.hpc.pomona.edu:/rhome/username/ \
  $BACKUP_DIR/

echo "Backup complete: $(du -sh $BACKUP_DIR)"
```

Automate with cron (every Sunday at 2 AM):
```bash
crontab -e
# Add: 0 2 * * 0 ~/sagehen_backup.sh >> ~/backup.log 2>&1
```

## Git for Code Backup

Use git for version-controlled code backup:

```bash
cd /rhome/username/myproject
git init
git add code/
git commit -m "Initial project"
git remote add origin https://github.com/username/myproject.git
git push -u origin main
```

## Archiving Completed Projects

When a project is finished, compress and move it:

```bash
# Compress the project
cd /rhome/username/projects
tar czf project_alpha.tar.gz project_alpha/

# Move to lab archive
mv project_alpha.tar.gz /bigdata/labname/archive/project_alpha_20260305.tar.gz

# Verify archive exists, then remove original
ls -lh /bigdata/labname/archive/project_alpha_20260305.tar.gz
rm -rf /rhome/username/projects/project_alpha/

# Check quota improvement
quota_check.sh
```

To restore an archived project:
```bash
cd /rhome/username/projects
tar xzf /bigdata/labname/archive/project_alpha_20260305.tar.gz
```

## Safe Data Migration

When moving data between storage locations:

```bash
# Step 1: Dry run
rsync -avhPr --dry-run /rhome/username/old_project/ /bigdata/labname/old_project/

# Step 2: Transfer
rsync -avhPr /rhome/username/old_project/ /bigdata/labname/old_project/

# Step 3: Verify sizes match
du --apparent-size -sh /rhome/username/old_project/
du --apparent-size -sh /bigdata/labname/old_project/

# Step 4: Verify file count
find /rhome/username/old_project/ -type f | wc -l
find /bigdata/labname/old_project/ -type f | wc -l

# Step 5: Delete source only after verification
rm -rf /rhome/username/old_project/
```

## Data Lifecycle Planning

Research data moves through stages:

```
Active Development  -->  Active Research  -->  Archive  -->  Preservation
    /rhome                /scratch for         /bigdata       External
    /bigdata              computation          compressed     repository
    (months)              (6-24 months)        (1-2 years)    (indefinite)
```

Plan retention for each project: what to keep, where to store it, and when to archive or publish.

::::::::::::::::::::::::::::::::::::: challenge

## Challenge: Create a Migration Plan

You need to move a 50 GB project from `/rhome/username/old_project/` to `/bigdata/labname/archive/`. Write the commands you would use, including:

1. Verify source size
2. Dry-run the transfer
3. Execute the transfer
4. Verify destination matches source
5. Safely remove the source

::::::::::::::::::::::::::::::::::::: solution

```bash
# 1. Verify source size
du --apparent-size -sh /rhome/username/old_project/

# 2. Dry run
rsync -avhPr --dry-run /rhome/username/old_project/ /bigdata/labname/archive/old_project/

# 3. Transfer
rsync -avhPr /rhome/username/old_project/ /bigdata/labname/archive/old_project/

# 4. Verify (compare size and file count)
du --apparent-size -sh /rhome/username/old_project/
du --apparent-size -sh /bigdata/labname/archive/old_project/
find /rhome/username/old_project/ -type f | wc -l
find /bigdata/labname/archive/old_project/ -type f | wc -l

# 5. Remove source (only after verification passes)
rm -rf /rhome/username/old_project/
quota_check.sh  # Confirm quota freed
```

Always verify before deleting. If sizes or file counts do not match, investigate before removing the source.

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Maintain local backups of Sagehen data using rsync scripts
- Use git for version-controlled backup of code
- Archive completed projects by compressing and moving to `/bigdata`
- Always verify size and file count before deleting the source after migration
- Plan data retention across the active, archive, and preservation stages

::::::::::::::::::::::::::::::::::::::::::::::::::
