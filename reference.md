# Quick Reference Guide

## Storage Locations

| Path | Type | Quota | Persistence | Speed | Use For |
|------|------|-------|-------------|-------|---------|
| `/rhome/<myusername>` | Home | Lab quota | Permanent | Normal | Personal files, home code |
| `/bigdata/lab/<labname>` | Lab shared | 1TB per lab | Permanent | Normal | Shared lab data |
| `/scratch/$SLURM_JOB_ID` | Job temp | Unlimited | Deleted at job end | Very fast | Job working data |
| `/tmpfs/$SLURM_JOB_ID` | RAM temp | Memory limit | Job lifetime | Fastest | In-memory working data |

## Quick Commands

### Check Storage Usage

```bash
# Check official quota
quota_check.sh

# Check directory size (accurate for BeeGFS)
du --apparent-size -sh /rhome/<myusername>

# Find largest files
find /rhome/<myusername> -type f -printf '%s %p\n' | sort -rn | head -10
```

### File Transfer Commands

```bash
# Upload single file
rsync -avhP local_file user@sagehen:/rhome/<myusername>/

# Upload directory
rsync -avhPr ~/mydir/ user@sagehen:/rhome/<myusername>/mydir/

# Download file
rsync -avhP user@sagehen:/rhome/<myusername>/file.dat ~/

# Download directory
rsync -avhPr user@sagehen:/rhome/<myusername>/mydir/ ~/mydir/

# Sync (incremental, only changed files)
rsync -avhPr ~/mydir/ user@sagehen:/rhome/<myusername>/mydir/

# Exclude patterns
rsync -avhPr --exclude='*.pyc' --exclude='.git' ~/mydir/ user@sagehen:/rhome/<myusername>/mydir/

# Dry run (preview what would transfer)
rsync -avhPr --dry-run ~/mydir/ user@sagehen:/rhome/<myusername>/mydir/

# With compression (slow networks)
rsync -avhPrz ~/mydir/ user@sagehen:/rhome/<myusername>/mydir/

# Bandwidth limit (10 MB/s)
rsync -avhPr --bwlimit=10m ~/mydir/ user@sagehen:/rhome/<myusername>/mydir/

# Delete files on remote not in local (CAREFUL!)
rsync -avhPr --delete ~/mydir/ user@sagehen:/rhome/<myusername>/mydir/
```

### OnDemand Access

```
URL: https://ondemand.sagehen.hpc.pomona.edu
Login: Pomona credentials + Duo
Navigate: Files → Browse /rhome/<myusername> or /bigdata/lab/<labname>
Upload: Click Upload button
Download: Right-click file → Download
```

### FileZilla Connection

```
Host: sagehen.hpc.pomona.edu
Port: 22
Protocol: SFTP
Username: your_pomona_username
Password: your_pomona_password
Duo: Approve on phone when prompted
```

### SLURM Job Variables

```bash
# In job scripts, use these SLURM variables:
$SLURM_JOB_USER         # Your username
$SLURM_JOB_ID           # Unique job ID
$SLURM_JOB_NAME         # Job name from #SBATCH
$SLURM_CPUS_ON_NODE     # Number of CPUs assigned
$SLURM_MEM_PER_NODE     # Memory per node

# Scratch path in jobs
SCRATCH=/scratch/$SLURM_JOB_USER/$SLURM_JOB_ID

# Tmpfs path in jobs
TMPFS=/tmpfs/$SLURM_JOB_USER/$SLURM_JOB_ID
```

## Job Script Patterns

### Copy Data In, Compute, Copy Out

```bash
#!/bin/bash
#SBATCH --job-name=analysis
#SBATCH --time=04:00:00

SCRATCH=/scratch/$SLURM_JOB_USER/$SLURM_JOB_ID
mkdir -p $SCRATCH

# Copy input
rsync -avhP /rhome/<myusername>/input.dat $SCRATCH/

# Compute
cd $SCRATCH
./myanalysis input.dat

# Copy results
rsync -avhP output.dat /rhome/<myusername>/results/
```

### Use Tmpfs for In-Memory Data

```bash
#!/bin/bash
#SBATCH --time=01:00:00
#SBATCH --mem=64G

TMPFS=/tmpfs/$SLURM_JOB_USER/$SLURM_JOB_ID
mkdir -p $TMPFS

# Work in tmpfs
cd $TMPFS
./analysis

# Copy results before job ends!
cp results.txt /rhome/<myusername>/
```

## Disk Usage Examples

### Find large files

```bash
# Top 10 largest files
find /rhome/<myusername> -type f -exec ls -lh {} \; | sort -k5 -h | tail -10

# Files larger than 100 MB
find /rhome/<myusername> -size +100M -type f
```

### Estimate directory size

```bash
# Home directory
du --apparent-size -sh /rhome/<myusername>

# Lab storage
du --apparent-size -sh /bigdata/lab/<labname>

# With breakdown
du --apparent-size -sh /rhome/<myusername>/* | sort -h
```

### Monitor quota in real-time

```bash
# Check every 5 seconds
watch -n 5 quota_check.sh
```

## Common rsync Flags

| Flag | Meaning |
|------|---------|
| `-a` | Archive mode (preserve permissions, timestamps) |
| `-v` | Verbose (show what's happening) |
| `-h` | Human-readable sizes |
| `-P` | Progress and partial (resume) |
| `-r` | Recursive (directories) |
| `-z` | Compression (slower CPU, saves bandwidth) |
| `--delete` | Delete remote files not in source (DANGEROUS!) |
| `--dry-run` | Preview without transferring |
| `--exclude='*.pyc'` | Skip matching files |
| `--bwlimit=10m` | Limit to 10 MB/s |

## Transfer Method Selection

```
File size < 50 MB
  → OnDemand (easiest)

File size 50 MB - 1 GB
  → FileZilla or rsync

File size > 1 GB
  → rsync (faster, resumable)

Many files (100+) or batch
  → rsync

Interactive browsing
  → FileZilla or OnDemand

Automated/scripted
  → rsync
```

## Data Organization Template

```
/rhome/<myusername>/
├── projects/
│   ├── project_name/
│   │   ├── data/
│   │   │   ├── raw/
│   │   │   └── processed/
│   │   ├── code/
│   │   ├── results/
│   │   └── README.md
│   └── other_project/
├── archive/
│   └── old_projects/
└── shared_resources/

/bigdata/lab/<labname>/
├── collaborative_projects/
├── lab_resources/
└── archive/
```

## Backup Commands

### Local backup

```bash
# Weekly backup of home directory
rsync -avhPr --exclude='.git' \
  user@sagehen:/rhome/<myusername>/ \
  ~/backups/sagehen_backup_$(date +%Y%m%d)/
```

### Archive completed project

```bash
# Compress
tar czf project_2024.tar.gz /rhome/<myusername>/project_name/

# Move to archive
mv project_2024.tar.gz /bigdata/lab/<labname>/archive/

# Extract when needed
tar xzf /bigdata/lab/<labname>/archive/project_2024.tar.gz
```

## Troubleshooting Commands

### Cannot connect via SSH

```bash
# Test connection
ping sagehen.hpc.pomona.edu

# Check SSH
ssh -v <myusername>@sagehen.hpc.pomona.edu
# -v shows verbose output for debugging
```

### File transfer fails

```bash
# Verify remote file exists
ssh <myusername>@sagehen.hpc.pomona.edu ls -lh /rhome/<myusername>/file.dat

# Check quota
ssh <myusername>@sagehen.hpc.pomona.edu quota_check.sh

# Try rsync with increased verbosity
rsync -vvhP file.dat user@sagehen:/rhome/<myusername>/
```

### Quota issues

```bash
# Check official quota
quota_check.sh

# Find what's using space
du --apparent-size -sh /rhome/<myusername>/* | sort -h

# Find old files
find /rhome/<myusername> -atime +90 -type f

# Find large files
find /rhome/<myusername> -size +1G -type f
```

## Performance Tips

- Use `/scratch` during jobs for 4-20x faster I/O
- Rsync is faster than FileZilla for large files
- Rsync incremental sync only transfers changed files
- Compression helps on slow networks
- Use bandwidth limits to avoid congestion

## Support and Help

- **HPC Support:** its-hpc@pomona.edu
- **IT Help:** servicedesk@pomona.edu
- **Emergency:** (during business hours) Pomona IT Help Desk
- **Documentation:** Workshop materials and setup.md

## Contact Information

- **HPC Coordinator:** Andrew Wilson
- **Email:** its-hpc@pomona.edu
- **Office Hours:** Posted at HPC portal

## Related Resources

- Episode 1: Storage Hierarchy
- Episode 2: Quotas and Management
- Episode 3: OnDemand Transfers
- Episode 4: FileZilla SFTP
- Episode 5: rsync CLI
- Episode 6: Temporary Storage in Jobs
- Episode 7: Best Practices

---

**Last Updated:** March 5, 2026
**Sagehen HPC Cluster:** BeeGFS parallel filesystem
**License:** CC-BY 4.0

<!-- highlight <labname>/<myusername> placeholders in code blocks; remove if the varnish theme handles this natively -->
<script>(function(){var CSS='.sh-placeholder{color:#c2410c;font-weight:700}[data-bs-theme="dark"] .sh-placeholder,html.dark .sh-placeholder{color:#fdba74}@media (prefers-color-scheme: dark){[data-bs-theme="auto"] .sh-placeholder{color:#fdba74}}';var RX=/<labname>|<myusername>/g;function firstMatch(el){var w=document.createTreeWalker(el,NodeFilter.SHOW_TEXT,null),nodes=[],full='';while(w.nextNode()){nodes.push({n:w.currentNode,s:full.length});full+=w.currentNode.nodeValue;}RX.lastIndex=0;var m;while((m=RX.exec(full))){var s=m.index,e=s+m[0].length,inSpan=false,parts=[];for(var j=0;j<nodes.length;j++){var ns=nodes[j].s,ne=ns+nodes[j].n.nodeValue.length;if(ne<=s||ns>=e)continue;parts.push({node:nodes[j].n,a:Math.max(s-ns,0),b:Math.min(e-ns,nodes[j].n.nodeValue.length)});var p=nodes[j].n.parentNode;while(p&&p!==el){if(p.classList&&p.classList.contains('sh-placeholder')){inSpan=true;break;}p=p.parentNode;}}if(!inSpan&&parts.length)return parts;}return null;}function wrapParts(parts){for(var i=parts.length-1;i>=0;i--){var t=parts[i].node,txt=t.nodeValue,a=parts[i].a,b=parts[i].b;var span=document.createElement('span');span.className='sh-placeholder';span.textContent=txt.slice(a,b);var f=document.createDocumentFragment();if(a>0)f.appendChild(document.createTextNode(txt.slice(0,a)));f.appendChild(span);if(b<txt.length)f.appendChild(document.createTextNode(txt.slice(b)));t.parentNode.replaceChild(f,t);}}function run(){var st=document.createElement('style');st.textContent=CSS;document.head.appendChild(st);document.querySelectorAll('pre,code').forEach(function(el){var guard=0,parts;while((parts=firstMatch(el))&&guard++<500){wrapParts(parts);}});}if(document.readyState==='loading'){document.addEventListener('DOMContentLoaded',run);}else{run();}})();</script>
