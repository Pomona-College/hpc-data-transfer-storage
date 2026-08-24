---
title: 'Instructor Notes'
---

## Workshop Overview

**Title:** Data Transfer and Storage Management on Sagehen HPC
**Duration:** 3-4 hours (including breaks)
**Target Audience:** New and intermediate HPC users
**Learning Outcomes:** Learners can efficiently transfer data, manage storage quotas, and choose appropriate storage locations for different use cases.

## Pre-Workshop Checklist

- [ ] Verify all learners can SSH into Sagehen (test connections 15 minutes before start)
- [ ] Have a test account ready for demos (e.g., `demo-user`)
- [ ] Test FileZilla connection before the workshop
- [ ] Test all command examples in your terminal
- [ ] Have the Sagehen documentation open: https://sagehen.hpc.pomona.edu/docs/
- [ ] Prepare example datasets on `/bigdata/` for demonstrations
- [ ] Set up a second monitor or large screen to share code/terminal output clearly
- [ ] Have a backup plan if network connectivity issues occur (pre-recorded demos)

## Episode-by-Episode Teaching Guide

### Episode 1: Understanding Sagehen's Storage Hierarchy (45 min teaching + 15 min exercises)

**Key Concepts:**
- Four storage locations: `/rhome/`, `/bigdata/`, `/scratch/`, `/tmpfs/`
- Persistent vs. temporary storage
- Performance tiers and use cases
- BeeGFS filesystem architecture

**Teaching Approach:**

1. **Open with a scenario** (~5 min):
   > "You've just started a large simulation that will generate 500 GB of intermediate data. Where do you save it? If you save to `/rhome/`, you'll immediately exceed your quota. If you save to `/scratch/`, it disappears when your job ends. What do you do?"

   This motivates the importance of understanding storage hierarchy.

2. **Live demonstration** (~20 min):
   - Log into Sagehen and navigate through each storage location
   - Show directory structure: `ls -lah /rhome/` and `ls -lah /bigdata/`
   - Run `df -h` to show storage usage and available space
   - Demonstrate `du -sh` to check folder sizes
   - Show `/scratch/$SLURM_JOB_ID` and `/tmpfs/$SLURM_JOB_ID` with a running SLURM job

3. **Interactive discussion** (~10 min):
   - Ask: "Which storage would you use for...?" (give examples like raw data, working files, final results)
   - Guide learners to understand trade-offs: persistence vs. speed vs. quota

4. **Exercises** (~15 min):
   - Have learners SSH into Sagehen and explore each storage location
   - Ask them to determine which partition they're on and its purpose
   - Have them check their quota and understand the output

**Common Issues & Solutions:**

| Issue | Solution |
|-------|----------|
| Learners can't SSH into Sagehen | Have them pair with someone who can; demo from your screen |
| DUO MFA timeouts | Show alternate authentication methods (SSH keys) |
| Quota command doesn't show lab data | Explain that `/bigdata/` quota is lab-shared; use `du -sh /bigdata/lab/<labname>/` |
| Confusion about `/scratch/$SLURM_JOB_ID` | Emphasize that this only exists during a job; submit a dummy job to show it |

**Timing Tips:**
- If running behind, skip some hands-on exploration and focus on concepts
- If time allows, show a sample job script that uses `/scratch/` and `/tmpfs/` effectively

---

### Episode 2: Managing Storage Quotas (30 min teaching + 20 min exercises)

**Key Concepts:**
- Home quota vs. lab quota
- Grace periods and quota enforcement
- Identifying large files and removing them
- Archive and compress strategies

**Teaching Approach:**

1. **Explain quota model** (~10 min):
   - Personal home quota: 100 GB (shared across all systems a user might access)
   - Lab shared quota: 1 TB per lab (shared across all lab members)
   - Grace period: 7 days to get below limit before hard enforcement
   - Show real quota output and explain fields

2. **Live demonstration** (~15 min):
   - Run `quota -s` and decode the output together
   - Show how to find large files: `find /rhome/<myusername> -type f -size +100M`
   - Demonstrate archiving: `tar -czf old_project.tar.gz old_project/`
   - Show compression savings: `du -sh old_project/` vs `du -sh old_project.tar.gz`
   - Demonstrate removal of temporary files

3. **Discussion: Best practices** (~5 min):
   - Encourage regular cleanup
   - Discuss archiving old projects
   - Mention communicating with lab if `/bigdata/` is full

4. **Exercises** (~20 min):
   - Have learners check their own quota
   - Find their three largest directories
   - Archive one test folder and compare sizes
   - Estimate how much space they could free up

**Common Issues & Solutions:**

| Issue | Solution |
|-------|----------|
| "I didn't get grace period notice" | Explain quota checks may not email; check `quota -s` regularly |
| Can't delete lab data (permissions) | Explain that only lab PI or designated admin can manage `/bigdata/` |
| Unsure what to delete | Recommend archiving before deleting; keep archives as backup |
| Archive is too slow | Use `--fast-read` with tar, or compress in background with `nohup` |

**Timing Tips:**
- Demo finding large files even if learners don't do hands-on (~5 min demo might save 10 min of exercises)
- If learners are slow with tar, provide a pre-compressed archive they can examine

---

### Episode 3: Transferring Data via OnDemand (40 min teaching + 20 min exercises)

**Key Concepts:**
- Web-based file transfer without command line
- GUI file browser on Sagehen
- When to use OnDemand vs. command-line tools
- Limitations of OnDemand for large transfers

**Teaching Approach:**

1. **Why OnDemand?** (~5 min):
   > "Not everyone is comfortable with the command line. OnDemand gives you a familiar, point-and-click interface. But it has limits: it's best for small to medium files (< 1 GB)."

2. **Live demonstration** (~25 min):
   - Navigate to https://ondemand.hpc.pomona.edu/ (show login with Pomona AD)
   - Click "Files" → "Home Directory" and navigate file structure
   - Upload a small test file
   - Download a file from Sagehen to local machine
   - Show creating new folders, renaming, deleting files
   - Demonstrate limitations: try uploading a large file, show the progress bar and estimate time
   - Open a terminal from OnDemand and show the underlying filesystem

3. **Compare methods** (~5 min):
   - Table: OnDemand vs SCP vs Rsync (speed, reliability, ease of use)
   - When to use each: GUI for exploration, CLI for reproducibility and large transfers

4. **Exercises** (~20 min):
   - Have learners log into OnDemand
   - Upload a test file from their computer
   - Create a new folder in their home directory
   - Download a file from Sagehen using OnDemand
   - Have them estimate how long a 1 GB file would take to transfer

**Common Issues & Solutions:**

| Issue | Solution |
|-------|----------|
| OnDemand login fails | Check if they're on campus/VPN; verify Pomona AD credentials |
| Browser doesn't support file drag-and-drop | Show alternative: click upload button and browse locally |
| Large file upload is slow | Explain this is expected; recommend rsync for large transfers |
| Can't see `/bigdata/` in OnDemand | Show how to navigate to `/bigdata/lab/<labname>/` using path bar |

**Timing Tips:**
- If OnDemand is unavailable (rare), skip and jump to SCP/Rsync; OnDemand is nice-to-have
- Have a pre-recorded demo ready as backup

---

### Episode 4: SFTP and FileZilla (45 min teaching + 15 min exercises)

**Key Concepts:**
- SFTP as a secure alternative to FTP
- FileZilla GUI setup and basic operations
- Key-based vs. password authentication
- Drag-and-drop file transfers

**Teaching Approach:**

1. **Why FileZilla?** (~5 min):
   > "FileZilla is one of the most user-friendly tools for transferring files with a server. It supports SFTP (secure), shows two-pane file browsing, and allows drag-and-drop."

2. **Installation and setup** (~10 min):
   - Show installation steps for Windows/macOS/Linux (learners should have done this in setup)
   - Troubleshoot any installation issues
   - Open FileZilla and show the interface

3. **Live demonstration** (~20 min):
   - Configure a new site:
     - Host: `sagehen.hpc.pomona.edu`
     - Protocol: SFTP
     - Port: 22
     - User: demo account
     - Password or SSH key
   - Connect and show the dual-pane interface (local on left, remote on right)
   - Navigate remote folders (show `/rhome/`, `/bigdata/`)
   - Drag a test file from local to remote
   - Show file properties (permissions, size)
   - Download a file from remote to local
   - Show transfer queue and logs
   - Demonstrate interrupted transfer (pause/resume)

4. **Best practices** (~5 min):
   - Use SSH keys instead of passwords for automated transfers
   - Watch transfer queue for errors
   - Use FileZilla for interactive transfers; use rsync/rclone for automated syncing

5. **Exercises** (~15 min):
   - Have learners set up FileZilla connection
   - Transfer a test file both directions
   - Navigate to `/bigdata/` and download a file
   - Have them note the transfer speed

**Common Issues & Solutions:**

| Issue | Solution |
|-------|----------|
| FileZilla says "Connection refused" | Check host name, port, and firewall; verify SSH is enabled on Sagehen |
| "Authentication failed" | Check username/password; try SSH key instead |
| Extremely slow transfer | Check network bandwidth; suggest rsync for bulk transfers |
| DUO prompt not appearing | Confirm DUO is set up; try SSH with `-v` flag to debug |

**Timing Tips:**
- If FileZilla installation takes too long, demo from your screen and provide installation guide
- Have a preconfigured FileZilla profile ready to share with learners

---

### Episode 5: Rsync and Command-Line Transfers (50 min teaching + 20 min exercises)

**Key Concepts:**
- Rsync incremental sync algorithm
- Efficiency: only transfers changed files
- Bandwidth and CPU optimization
- Resume interrupted transfers
- Exclude patterns and dry-runs

**Teaching Approach:**

1. **Why rsync over scp?** (~5 min):
   > "If you're transferring 100 GB and your connection drops after 99 GB, SCP restarts from the beginning. Rsync picks up where it left off. For large transfers, rsync is essential."

2. **Live demonstration** (~25 min):
   - Create a test dataset locally (mix of files, some large)
   - Show basic rsync: `rsync -av local_folder/ user@sagehen:/bigdata/lab/`
   - Explain flags: `-a` (archive), `-v` (verbose), `--progress` (show progress)
   - Run again to show "nothing to do" (efficient!)
   - Modify one file locally and re-run; show only that file transfers
   - Demonstrate `--dry-run` to preview what would be transferred
   - Show `--exclude` to skip certain files: `rsync -av --exclude '*.tmp' ...`
   - Demonstrate bandwidth limiting: `--bwlimit=10000` (useful for not saturating network)
   - Show transferring in reverse direction (download from Sagehen)
   - Demonstrate stopping and resuming (Ctrl-C, then re-run same command)

3. **Common patterns** (~10 min):
   - Show example for daily backup: `rsync -av --delete local/ user@sagehen:/bigdata/lab/backup/`
   - Show pattern for excluding build artifacts: `rsync -av --exclude='.git' --exclude='node_modules' code/ user@sagehen:/bigdata/lab/code/`
   - Discuss best practices: test with `--dry-run`, use descriptive progress

4. **Exercises** (~20 min):
   - Have learners create a local test folder with multiple files
   - Run rsync to transfer to Sagehen
   - Have them modify a file and re-run (show incremental sync)
   - Use `--dry-run` to preview a transfer with `--delete`
   - Transfer a folder from Sagehen back to local (reverse direction)

**Common Issues & Solutions:**

| Issue | Solution |
|-------|----------|
| "Rsync not found" on Windows | Recommend Git Bash or WSL; or use FileZilla instead |
| Slow rsync (checking permissions) | Add `-O` flag to skip group sync (context-specific) |
| Accidentally deleted remote with `--delete` | Remind that `--dry-run` should be used first; restore from snapshot if needed |
| Rsync hangs during transfer | Check network; show how to increase SSH timeout with `-e 'ssh -o ConnectTimeout=60'` |

**Timing Tips:**
- Rsync is critical; spend extra time here even if it means cutting other episodes short
- Have a large dataset prepared so learners see realistic transfer times
- Provide a cheat sheet of common rsync patterns

---

### Episode 6: Scratch and Temporary Storage (40 min teaching + 15 min exercises)

**Key Concepts:**
- `/scratch/` is job-local SSD storage (fast, temporary)
- `/tmpfs/` is RAM-backed storage (fastest, most limited)
- Both are automatically deleted when jobs end
- How to use in SLURM job scripts
- Copying data in and out of scratch

**Teaching Approach:**

1. **Why temporary storage?** (~5 min):
   > "Imagine processing a 500 GB dataset that produces 2 TB of intermediate files. `/rhome/` and `/bigdata/` share a single 1 TB lab quota on BeeGFS, which won't fit this workflow. `/scratch/` is designed exactly for this: it's fast (non-persistent SSD with a single shared pool), and is deleted when the job completes."

2. **Understanding `/scratch/$SLURM_JOB_ID`** (~10 min):
   - Show that `/scratch/` is only available within a SLURM job
   - Explain `$SLURM_JOB_ID` is an environment variable set by SLURM
   - Outside a job, `/scratch/` might not exist or be empty
   - Show daily cleanup: files are deleted after 30 days of no access
   - Demonstrate checking current job ID: `echo $SLURM_JOB_ID`

3. **SLURM job script patterns** (~15 min):
   - Show a simple job that uses `/scratch/`:
     ```bash
     #!/bin/bash
     #SBATCH -J analysis
     #SBATCH -o /rhome/<myusername>/logs/analysis.log

     # Copy input data to scratch
     cp /bigdata/lab/input.tar.gz /scratch/$SLURM_JOB_ID/
     cd /scratch/$SLURM_JOB_ID

     # Extract and process
     tar -xzf input.tar.gz
     ./process_data.sh

     # Copy results back to persistent storage
     cp results.tar.gz /rhome/<myusername>/results/

     # Scratch is cleaned up automatically when job ends
     ```
   - Explain each step: copy in, process, copy out
   - Discuss alternative: symlink to `/scratch/` if output must go to `/bigdata/`
   - Show monitoring disk usage: `df -h /scratch/$SLURM_JOB_ID` within job

4. **Live demonstration** (~10 min):
   - Submit a test job that writes to `/scratch/`
   - While job is running, log in from another terminal and show the `/scratch/` directory exists
   - Show file size growing with `du -sh /scratch/$SLURM_JOB_ID/`
   - Wait for job to finish and show `/scratch/` cleaned up or job-specific directory removed

5. **Exercises** (~15 min):
   - Have learners write a SLURM script that uses `/scratch/`
   - Submit the job and monitor progress
   - Check that their results were copied back correctly
   - Verify `/scratch/` was cleaned up

**Common Issues & Solutions:**

| Issue | Solution |
|-------|----------|
| `/scratch/` doesn't exist in job | This is normal if storage node is offline; file is written to temp local disk |
| Forgot to copy results back, job ended | Results are gone; learn lesson and always copy back. Mention recovery in extreme cases |
| Job output is huge; can't fit in `/scratch/` | Use `/tmpfs/` for intermediate files; stream output directly to persistent storage |
| `/tmpfs/` is too small | Explain it's RAM-backed; only suitable for small intermediate files; use `/scratch/` for large data |

**Timing Tips:**
- If learners are struggling with SLURM, focus on conceptual understanding of `/scratch/` and general job patterns
- Provide template job scripts so they can focus on the storage concepts

---

### Episode 7: Data Transfer Best Practices and Automation (40 min teaching + 15 min exercises)

**Key Concepts:**
- Choosing the right tool (SCP vs Rsync vs Rclone)
- Automating transfers with cron jobs
- Handling large datasets efficiently
- Monitoring and logging transfers
- Recovery from failures

**Teaching Approach:**

1. **Decision tree for tool selection** (~10 min):
   - "Is the data small (< 500 MB)?" → Use SCP or FileZilla
   - "Is it large and might be interrupted?" → Use Rsync
   - "Are you syncing with cloud storage?" → Use Rclone
   - "Does it need to run every night?" → Use Rsync in a cron job
   - Show a visual decision tree on slide or whiteboard

2. **Rclone for cloud sync** (~15 min):
   - Demonstrate configuring rclone (one-time setup)
   - Show syncing from Google Drive to Sagehen:
     ```bash
     rclone sync gdrive:/Datasets/experiment1 sagehen:/bigdata/lab/experiment1 --progress
     ```
   - Show reverse: backup to S3:
     ```bash
     rclone sync sagehen:/bigdata/lab/results s3://lab-backup/results --progress
     ```
   - Explain rclone is slower than rsync for local transfers but powerful for cloud

3. **Cron jobs for automated transfers** (~10 min):
   - Show cron syntax: `0 22 * * *` = "every night at 10 PM"
   - Write a sample script that syncs `/rhome/` to backup:
     ```bash
     #!/bin/bash
     rsync -av --log-file=/rhome/<myusername>/logs/sync.log /rhome/<myusername>/ /bigdata/lab/<labname>/backup/user_home/
     ```
   - Show how to schedule it: `crontab -e`
   - Discuss: "When should I sync? What if it fails?"

4. **Monitoring and logging** (~5 min):
   - Show `rsync --log-file=` to track transfers
   - Recommend `mail` or `slurm-mail` for notifications if transfer fails
   - Show how to check transfer logs: `tail -f sync.log`

5. **Recovery strategies** (~5 min):
   - Interrupted transfer: "just re-run rsync; it picks up where it left off"
   - Partial corruption: "use `--checksum` flag to verify integrity"
   - Network timeouts: "use SSH config to increase timeout"

6. **Exercises** (~15 min):
   - Have learners create a cron job (or at least write the script) that syncs a folder
   - Have them test a large transfer with `--dry-run` first
   - Have them add logging to the command
   - Discuss what they'd monitor for in production

**Common Issues & Solutions:**

| Issue | Solution |
|-------|----------|
| Cron job runs but produces no output | Show `--log-file` option; check crontab has right path |
| "Permission denied" in cron job | Check file ownership; cron runs as the user, but SSH keys must be accessible |
| Cloud storage sync is extremely slow | Rclone is slower than rsync; this is expected. For local-to-Sagehen, use rsync |
| Running out of disk during transfer | Use `--bwlimit` to slow transfer, buy time to free up space; or use incremental transfers |

**Timing Tips:**
- If running short on time, focus on rsync best practices; rclone and cron are bonus
- Provide ready-to-use cron script templates

---

## Common Learner Mistakes & How to Address Them

| Mistake | Correction | Teaching Tip |
|---------|-----------|--------------|
| Uploading directly to `/scratch/` | Explain `/scratch/` exists only during SLURM jobs; use `/rhome/` or `/bigdata/` for permanent storage | Show the error message they'd get |
| Running out of quota without understanding | Teach proactive monitoring with `quota -s`; emphasize checking regularly | Have them set a monthly calendar reminder |
| Transferring small files one-by-one | Teach tar/zip for archiving; show how bundling reduces overhead | Demonstrate speed comparison |
| Using SCP for multi-TB transfers (and it fails halfway) | This is the perfect "teaching moment": ask if they know what to do. Then teach rsync. | Show rsync's resume capability |
| Forgetting to copy results back from `/scratch/` | Emphasize "always copy back" in job scripts; show job-end cleanup section | Provide template with comments |
| Not using `--dry-run` with rsync `--delete` | Always recommend `--dry-run` first; make it a habit | Demo the danger of `--delete` |

## Assessment & Feedback

### Informal Checks (During Workshop)

- Ask: "Where would you store 500 GB of intermediate data?" (Should answer `/scratch/`)
- Ask: "How do you resume an interrupted transfer?" (Should mention rsync)
- Have them show you a `quota -s` output and interpret it
- Ask: "When would you use FileZilla vs. rsync?" (FileZilla for small/GUI, rsync for large/automated)

### Post-Workshop Feedback

- Provide a short survey: "Which topic would you like more practice with?"
- Offer optional follow-up sessions on automation or advanced rsync
- Share the reference guide and example scripts for future use

## Resource Materials

### For Instructors

- Sagehen HPC documentation: https://sagehen.hpc.pomona.edu/docs/
- Rsync man page: `man rsync` (or https://linux.die.net/man/1/rsync)
- FileZilla documentation: https://wiki.filezilla-project.org/
- Rclone documentation: https://rclone.org/

### For Learners (Reference Materials Provided)

- `/learners/setup.md` - Pre-workshop setup instructions
- `/learners/reference.md` - Quick reference for commands and use cases
- Example scripts (create in `/instructors/examples/` for sharing)

## Tips for Engagement

1. **Real-world motivation**: Use examples from your own research or institution's common workflows
2. **Let failures happen** (in demos): An interrupted transfer is a teaching moment
3. **Encourage questions**: Data transfer is a common pain point; learners have many questions
4. **Hands-on early**: Don't lecture for 30 minutes; get them on the command line quickly
5. **Provide templates**: Share working job scripts and rsync commands; let them modify
6. **Celebrate small wins**: "Your first rsync worked!" is motivating

## Instructor Logistics

- **Setup time**: Arrive 30 min early to test connections, load terminals, and troubleshoot
- **Backup plan**: Have screen recordings ready if live demos fail
- **Pair programming**: Pair learners with weak technical skills with more experienced ones
- **Slack/email**: Provide a way for learners to ask questions during and after
- **Follow-up**: Share slides, reference guide, and example scripts within a day

---

## Questions to Engage Learners

- "What's the biggest dataset you've had to transfer? How did you do it?"
- "What's the most frustrating part of data transfer for you?"
- "Why do you think persistent and temporary storage are separate?"
- "If you had to transfer 1 TB overnight, which tool would you choose and why?"
- "What would happen if you ran out of `/rhome/` quota mid-job?"

These personalize the workshop and help you tailor content to your audience.

<!-- highlight <labname>/<myusername> placeholders in code blocks; remove if the varnish theme handles this natively -->
<script>(function(){var CSS='.sh-placeholder{color:#c2410c;font-weight:700}[data-bs-theme="dark"] .sh-placeholder,html.dark .sh-placeholder{color:#fdba74}@media (prefers-color-scheme: dark){[data-bs-theme="auto"] .sh-placeholder{color:#fdba74}}';var RX=/<labname>|<myusername>/g;function firstMatch(el){var w=document.createTreeWalker(el,NodeFilter.SHOW_TEXT,null),nodes=[],full='';while(w.nextNode()){nodes.push({n:w.currentNode,s:full.length});full+=w.currentNode.nodeValue;}RX.lastIndex=0;var m;while((m=RX.exec(full))){var s=m.index,e=s+m[0].length,inSpan=false,parts=[];for(var j=0;j<nodes.length;j++){var ns=nodes[j].s,ne=ns+nodes[j].n.nodeValue.length;if(ne<=s||ns>=e)continue;parts.push({node:nodes[j].n,a:Math.max(s-ns,0),b:Math.min(e-ns,nodes[j].n.nodeValue.length)});var p=nodes[j].n.parentNode;while(p&&p!==el){if(p.classList&&p.classList.contains('sh-placeholder')){inSpan=true;break;}p=p.parentNode;}}if(!inSpan&&parts.length)return parts;}return null;}function wrapParts(parts){for(var i=parts.length-1;i>=0;i--){var t=parts[i].node,txt=t.nodeValue,a=parts[i].a,b=parts[i].b;var span=document.createElement('span');span.className='sh-placeholder';span.textContent=txt.slice(a,b);var f=document.createDocumentFragment();if(a>0)f.appendChild(document.createTextNode(txt.slice(0,a)));f.appendChild(span);if(b<txt.length)f.appendChild(document.createTextNode(txt.slice(b)));t.parentNode.replaceChild(f,t);}}function run(){var st=document.createElement('style');st.textContent=CSS;document.head.appendChild(st);document.querySelectorAll('pre,code').forEach(function(el){var guard=0,parts;while((parts=firstMatch(el))&&guard++<500){wrapParts(parts);}});}if(document.readyState==='loading'){document.addEventListener('DOMContentLoaded',run);}else{run();}})();</script>
