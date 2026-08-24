---
title: Learner Profiles
---

## Overview

The following learner profiles represent typical backgrounds and motivations for the Data Transfer and Storage Management workshop. Use these to tailor examples, pacing, and depth of technical content during the workshop.

---

## Profile 1: Graduate Student with Large Genomics Datasets

**Name:** Maria Chen
**Background:** PhD student in molecular biology
**Technical Level:** Intermediate (familiar with Linux basics)
**Motivation for Workshop:** Her lab has acquired 500 GB of raw genomic sequencing data, and she needs to process it on Sagehen.

### Current Challenges

- Unsure where to store raw data vs. intermediate files vs. final results
- Data is split across her laptop, lab server, and external hard drives: unorganized
- Previous experience transferring files over FTP (slow and unreliable)
- Has hit `/rhome/` quota limits before
- Worries about losing data during long-running analyses

### Specific Needs from Workshop

- How to efficiently transfer 500 GB to Sagehen without losing data mid-transfer
- Best practices for organizing genome analysis workflows across storage tiers
- How to monitor storage usage during long SLURM jobs
- Whether `/bigdata/` (shared lab storage) is appropriate for sensitive raw data
- How to keep intermediate files off `/bigdata/` to not take up lab quota

### Expected Outcomes

- Can confidently transfer multi-GB datasets using rsync
- Understands proper use of `/scratch/` for intermediate files in job scripts
- Can organize a complex research workflow across storage locations
- Can check and manage storage quota proactively

### Teaching Adjustments

- Use genomics as the running example (DNA sequencing, BAM files, VCF files)
- Show rsync with resume capability (critical for large transfers)
- Spend time on SLURM job script structure with `/scratch/` integration
- Discuss managing 1TB of data within a 1TB lab quota (prioritization, archiving)

---

## Profile 2: Faculty with Lab Data Sharing Needs

**Name:** Dr. James Okonkwo
**Background:** Associate Professor in Chemistry, running a 5-person lab
**Technical Level:** Low (primarily uses GUI tools; limited command-line experience)
**Motivation for Workshop:** Needs a systematic way to share research data among lab members without misconfiguring permissions or losing files.

### Current Challenges

- Lab members email data files to each other (disorganized, multiple versions)
- Unclear who has access to what on current lab server
- Some students have accidentally overwritten files
- Wants to transition to centralized storage on Sagehen but unsure how
- Has never used the command line or SSH before

### Specific Needs from Workshop

- A simple, reliable method to upload/download files to Sagehen
- How to set up `/bigdata/lab/<labname>/` for his lab team
- Ensuring his students don't accidentally overwrite each other's work
- Permission and access control for shared lab data
- Backup strategy for lab data (what if the server fails?)

### Expected Outcomes

- Can use FileZilla to transfer files graphically (no command line required)
- Understands `/bigdata/` as the lab's shared workspace and how to organize it
- Knows how to check if disk space is running low
- Can explain storage locations to graduate students starting in the lab
- Has a basic backup/archiving strategy

### Teaching Adjustments

- Start with FileZilla (graphical interface), minimal command line
- Focus on `/bigdata/` organization: folder structure, permissions, shared responsibility
- Use "lab data management" as the running example (not individual workflows)
- Include practical scenario: "Your student graduated; how do you preserve their work?"
- Provide templates for lab directory structure they can copy
- Show OnDemand as alternative to FileZilla for those uncomfortable with downloads
- Minimal discussion of rsync/automation; focus on interactive transfers

---

## Profile 3: Undergraduate Research Assistant New to HPC

**Name:** Alex Rodriguez
**Background:** Junior computer science student on summer internship
**Technical Level:** Low (knows Python, but new to HPC and clusters)
**Motivation for Workshop:** Starting summer research internship; supervisor recommended HPC for computations but Alex has no experience with remote systems.

### Current Challenges

- Has never used SSH or command line for remote connections
- Unfamiliar with concepts like "persistent vs. temporary storage"
- Worried about accidentally deleting files or exceeding quota
- Doesn't understand why there are multiple storage locations
- Comfortable with programming but not systems administration

### Specific Needs from Workshop

- A gentle introduction to HPC storage concepts (not overwhelming technical details)
- How to copy code and data to Sagehen for analysis
- Understanding quota limits so they don't block the lab
- Confidence to troubleshoot basic connection issues
- How to work within the framework set up by their supervisor

### Expected Outcomes

- Can SSH into Sagehen and navigate the filesystem
- Understands the purpose of each storage location and chooses correctly
- Can upload small datasets and Python scripts using FileZilla or SCP
- Knows to ask supervisor before using large amounts of `/bigdata/`
- Can interpret `quota -s` output and take action if running low
- Can run a SLURM job that saves results to persistent storage

### Teaching Adjustments

- Start very basic: "What is SSH? Why do we need it?"
- Use lots of analogies ("persistent storage is like your Google Drive; temporary storage is like a browser cache")
- Pair with a more experienced learner for hands-on sections
- Emphasize "ask your supervisor" for permission/policy questions
- Provide step-by-step guides for each tool (FileZilla, SCP, SSH)
- Show common mistakes and recovery strategies
- Celebrate small wins ("You just successfully transferred your first file to a supercomputer!")

---

## Profile 4: Visiting Researcher with Temporary Data Needs

**Name:** Dr. Sophie Arnault
**Background:** Postdoc from France collaborating on a 3-month research project
**Technical Level:** Intermediate-High (familiar with Linux and data transfer)
**Motivation for Workshop:** Visiting Pomona for a short research collaboration; needs to transfer large datasets from her home institution and sync them to Sagehen.

### Current Challenges

- Time zone differences make synchronous collaboration difficult
- Has already set up VPN but unfamiliar with Sagehen specifically
- Needs to transfer 2 TB of raw data from her home institution
- Will be leaving in 3 months; needs to understand data retention policies
- Uses both MacOS and Linux; wants cross-platform solutions

### Specific Needs from Workshop

- How to efficiently transfer 2 TB across continents with resume capability
- Setting up automated syncing between her home institution and Sagehen
- Understanding Sagehen's backup and data retention policies
- Organizing her temporary project on `/bigdata/` (or appropriate location)
- Archiving and moving results back to her home institution when leaving

### Expected Outcomes

- Can set up rsync for reliable multi-TB transfers with resume capability
- Understands `/bigdata/` quota and will clean up before leaving
- Can automate daily sync of home institution data to Sagehen
- Knows what data is backed up and what's temporary
- Can prepare a final archive of results for transfer back home

### Teaching Adjustments

- Deep dive into rsync: `--checksum`, `--bwlimit`, `--log-file`, resume strategies
- Discuss rclone for syncing if her home institution has cloud storage
- Include real-world scenario: "Your connection drops mid-transfer; what do you do?"
- Talk about data portability: what can she take when she leaves?
- Provide sample cron job for nightly syncing
- Fast-track through basic concepts; focus on advanced use cases
- Discuss `/tmpfs/` for short-term intensive computations

---

## Profile 5: Lab Manager Responsible for Data Administration

**Name:** Tom Walsh
**Background:** Lab manager for a computational biology group (not a researcher)
**Technical Level:** Intermediate-High (manages servers; knows shell scripting)
**Motivation for Workshop:** Needs to manage data storage, backups, and access for a 15-person lab using Sagehen.

### Current Challenges

- Lab has inconsistent data organization across projects
- No automated backup strategy; worried about data loss
- Students often ask "where should I save this?" with no standard answer
- Lab has hit `/bigdata/` quota limits multiple times
- Needs to audit who has access to sensitive project data
- Planning migration of legacy data to Sagehen

### Specific Needs from Workshop

- Best practices for lab data organization and structure
- Setting up automated backups/archiving to off-site storage (cloud or tape)
- Monitoring `/bigdata/` quota and alerting when approaching limit
- Documenting data management policies for lab members
- Auditing and controlling access to shared lab storage

### Expected Outcomes

- Has a documented lab data management policy
- Can set up automated rsync or rclone jobs for backup
- Can create lab folder structure and permission scheme
- Can write scripts to monitor quota and send alerts
- Can train other lab members on proper data practices
- Understands Sagehen's backup policies and plans accordingly

### Teaching Adjustments

- Fast-track through basic storage concepts; focus on automation and scripting
- Provide sample scripts for quota monitoring and alerting
- Discuss rclone for cloud backups (S3, Glacier, Google Drive)
- Cover permission management and access control
- Include scenario: "How do you preserve a graduated student's data?"
- Provide templates for lab data management documentation
- Discuss retention policies: what data to keep long-term vs. archive/delete

---

## Using These Profiles

### During Workshop Planning

- Anticipate questions from each profile type
- Prepare examples that resonate (genomics for Maria, shared access for James, etc.)
- Plan breakout sessions for different levels (beginners vs. advanced)

### During Teaching

- Reference profiles to answer "Who might have this problem?"
- Adjust pacing: move faster for Tom (lab manager), slower for Alex (undergrad)
- Use real scenarios from profiles: "Maria's in the room: let's talk about 500 GB transfers"

### For Differentiated Exercises

- **Beginner (Alex, James):** FileZilla transfer, quota check, navigate folders
- **Intermediate (Maria, Sophie):** Rsync with resume, SLURM job with `/scratch/`, cron jobs
- **Advanced (Tom):** Automated backups, monitoring scripts, lab-scale policies

---

## Demographic Mix and Pacing

If your workshop has a mix of these profiles:

- **Mostly beginners (Alex, James):** Spend 50% on FileZilla, 30% on rsync, 20% on concepts
- **Mostly experienced (Maria, Tom):** Spend 20% on concepts, 30% on rsync, 30% on automation, 20% on Sagehen specifics
- **Mixed group:** Use breakout exercises; pair advanced learners with beginners; provide optional advanced material

---

## Follow-Up and Support

After the workshop, tailor follow-up resources:

- **For Alex & James:** Provide detailed step-by-step guides, video tutorials for FileZilla
- **For Maria:** Share genomics-specific workflow examples, SLURM job templates
- **For Sophie:** Provide rsync templates, timezone-friendly communication for collaboration
- **For Tom:** Share monitoring/backup scripts, lab policy templates

All learners should receive:
- Reference guide with commands
- Contact information: its-hpc@pomona.edu
- Sagehen documentation link
- Example scripts repository

<!-- highlight <labname>/<myusername> placeholders in code blocks; remove if the varnish theme handles this natively -->
<script>(function(){var CSS='.sh-placeholder{color:#c2410c;font-weight:700}[data-bs-theme="dark"] .sh-placeholder,html.dark .sh-placeholder{color:#fdba74}@media (prefers-color-scheme: dark){[data-bs-theme="auto"] .sh-placeholder{color:#fdba74}}';var RX=/<labname>|<myusername>/g;function firstMatch(el){var w=document.createTreeWalker(el,NodeFilter.SHOW_TEXT,null),nodes=[],full='';while(w.nextNode()){nodes.push({n:w.currentNode,s:full.length});full+=w.currentNode.nodeValue;}RX.lastIndex=0;var m;while((m=RX.exec(full))){var s=m.index,e=s+m[0].length,inSpan=false,parts=[];for(var j=0;j<nodes.length;j++){var ns=nodes[j].s,ne=ns+nodes[j].n.nodeValue.length;if(ne<=s||ns>=e)continue;parts.push({node:nodes[j].n,a:Math.max(s-ns,0),b:Math.min(e-ns,nodes[j].n.nodeValue.length)});var p=nodes[j].n.parentNode;while(p&&p!==el){if(p.classList&&p.classList.contains('sh-placeholder')){inSpan=true;break;}p=p.parentNode;}}if(!inSpan&&parts.length)return parts;}return null;}function wrapParts(parts){for(var i=parts.length-1;i>=0;i--){var t=parts[i].node,txt=t.nodeValue,a=parts[i].a,b=parts[i].b;var span=document.createElement('span');span.className='sh-placeholder';span.textContent=txt.slice(a,b);var f=document.createDocumentFragment();if(a>0)f.appendChild(document.createTextNode(txt.slice(0,a)));f.appendChild(span);if(b<txt.length)f.appendChild(document.createTextNode(txt.slice(b)));t.parentNode.replaceChild(f,t);}}function run(){var st=document.createElement('style');st.textContent=CSS;document.head.appendChild(st);document.querySelectorAll('pre,code').forEach(function(el){var guard=0,parts;while((parts=firstMatch(el))&&guard++<500){wrapParts(parts);}});}if(document.readyState==='loading'){document.addEventListener('DOMContentLoaded',run);}else{run();}})();</script>
