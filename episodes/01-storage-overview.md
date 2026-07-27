---
title: "Storage Architecture Overview"
teaching: 20
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- What storage options are available on Sagehen?
- Where should I store different types of data?
- What are the differences between persistent and temporary storage?

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

After completing this episode, participants will be able to:
- Describe the four main storage locations on Sagehen
- Explain the persistence and availability of each filesystem
- Choose the appropriate storage location for different use cases

::::::::::::::::::::::::::::::::::::::::::::::::::

## Sagehen's Storage System

Sagehen uses a **hierarchical storage system** designed to balance performance, reliability, and cost. The four main storage locations are:

1. **`/rhome/username`** -- Home directory (persistent, lab-shared quota)
2. **`/bigdata/labname`** -- Lab shared storage (persistent, 1TB quota per lab)
3. **`/scratch/$SLURM_JOB_ID`** -- Job-local temporary SSD (deleted when job completes)
4. **`/tmpfs/$SLURM_JOB_ID`** -- RAM-backed temporary storage (deleted when job completes)

```
Sagehen Storage Architecture
+-- Persistent Storage (survives job termination)
|   +-- /rhome/username       (Home, BeeGFS, weekly snapshots)
|   +-- /bigdata/labname      (Lab shared, BeeGFS, weekly snapshots)
|
+-- Temporary Storage (deleted when job completes)
    +-- /scratch/$SLURM_JOB_ID  (SSD-backed, very fast)
    +-- /tmpfs/$SLURM_JOB_ID    (RAM-backed, fastest)
```

::::::::::::::::::::::::::::::::::::: callout

## Storage Performance Tiers

- **Fastest**: `/tmpfs` (RAM-backed, node-local)
- **Very Fast**: `/scratch` (SSD-backed)
- **Standard**: `/rhome` and `/bigdata` (network storage)

Use temporary storage for intermediate data during jobs to improve performance 5-10x.

::::::::::::::::::::::::::::::::::::::::::::::::::

## Persistent Storage

### Home Directory: `/rhome/username`

Your personal storage space, accessible from all nodes and OnDemand.

- **Quota**: Shared with lab (included in 1TB lab quota)
- **Persistence**: Permanent
- **Backup**: Weekly snapshots (contact its-hpc@pomona.edu for recovery)
- **Filesystem**: BeeGFS distributed filesystem
- **Best for**: Source code, config files, small datasets, job scripts

### Lab Shared Storage: `/bigdata/labname`

Shared storage for your entire lab group.

- **Quota**: 1TB per lab (shared with `/rhome` across all members)
- **Persistence**: Permanent
- **Backup**: Weekly snapshots
- **Best for**: Large shared datasets, collaborative data, archived projects

::::::::::::::::::::::::::::::::::::: callout

## Shared Quota Responsibility

`/rhome` and `/bigdata` share a single 1TB lab quota. One person filling up the quota affects everyone. Coordinate with lab members about large data storage and archive old projects regularly. Request quota increases by contacting its-hpc@pomona.edu.

::::::::::::::::::::::::::::::::::::::::::::::::::

## Temporary Storage

### Scratch: `/scratch/$SLURM_JOB_USER/$SLURM_JOB_ID`

Fast SSD-backed storage for intermediate data during job execution. Deleted automatically when your job completes. Use for input staging, intermediate results, and checkpoint files.

### Tmpfs: `/tmpfs/$SLURM_JOB_USER/$SLURM_JOB_ID`

Fastest storage available, backed by node RAM. Deleted when job terminates. Limited by job memory allocation and restricted to a single node. Use for rapid read-write cycles and small working datasets.

## Choosing the Right Location

```
Will this data survive beyond my current job?
    +-- YES
    |   +-- Will lab members need access?
    |       +-- YES --> /bigdata/labname
    |       +-- NO  --> /rhome/username
    +-- NO (temporary data)
        +-- Does data fit in allocated memory?
            +-- YES --> /tmpfs (fastest)
            +-- NO  --> /scratch (very fast)
```

## BeeGFS Filesystem

All Sagehen storage uses BeeGFS, a parallel distributed filesystem. Key implication: the standard `du` command may be inaccurate. Always use `du --apparent-size` or the `quota_check.sh` script for accurate measurements.

::::::::::::::::::::::::::::::::::::: challenge

## Challenge: Storage Selection

For each scenario, choose the best storage location and justify your answer:

1. A 500MB raw data file that your entire lab will analyze
2. A 4-hour job processing 100GB of data and generating 50GB of results
3. Python scripts and notebooks for future reference
4. A job using 48GB of memory that needs a temporary 40GB working directory

::::::::::::::::::::::::::::::::::::: solution

1. **`/bigdata/labname`** -- Needs lab-wide access and persistence
2. **`/scratch`** for working data, copy results back to `/rhome` -- Fast I/O, too large for RAM
3. **`/rhome/username`** -- Personal, small, needs permanent persistence
4. **`/tmpfs`** -- Fits in allocated memory, fastest possible I/O for single-node work

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Sagehen provides four storage locations with different persistence and performance characteristics
- `/rhome` and `/bigdata` are persistent and share a 1TB lab quota on BeeGFS
- `/scratch` and `/tmpfs` are temporary, deleted when the job completes
- Choose storage based on persistence needs, access patterns, and performance requirements

::::::::::::::::::::::::::::::::::::::::::::::::::
