# Data Transfer and Storage Management on Sagehen HPC Cluster

Welcome to Workshop 12: Data Transfer and Storage Management for Pomona College's Sagehen HPC cluster.

## Overview

This Carpentries Workbench workshop teaches researchers how to efficiently manage data on the Sagehen HPC cluster. Through seven comprehensive episodes, you'll learn storage locations, file transfer methods, quota management, and best practices for organizing research data.

**Duration:** 4-5 hours
**Target Audience:** Sagehen users of all experience levels
**Prerequisites:** Active Sagehen account, basic Linux familiarity

## Workshop Contents

### Episode 1: Storage Hierarchy (45 min + 15 min exercises)
Understand the four main storage locations on Sagehen and when to use each:
- `/rhome/<myusername>` - Home directory (persistent)
- `/bigdata/lab/<labname>` - Lab shared storage (persistent, 1TB quota)
- `/scratch/$SLURM_JOB_ID` - Job temporary SSD storage
- `/tmpfs/$SLURM_JOB_ID` - RAM-backed temporary storage

### Episode 2: Quotas and Management (45 min + 15 min exercises)
Learn to monitor and manage your storage allocation:
- Using `quota_check.sh`
- Understanding BeeGFS filesystem
- Finding and removing large files
- Planning for quota limits

### Episode 3: OnDemand Web Interface (40 min + 20 min exercises)
Transfer files using the browser-based interface:
- Accessing OnDemand
- Uploading and downloading files
- Organizing files through web interface
- When to use OnDemand

### Episode 4: FileZilla SFTP Client (50 min + 20 min exercises)
Use a graphical tool for interactive file transfers:
- Installing and configuring FileZilla
- Drag-and-drop file transfers
- Managing connections
- Handling Duo authentication

### Episode 5: rsync Command-Line (60 min + 25 min exercises)
Master the fastest bulk transfer tool:
- rsync basics and advantages
- Uploading and downloading directories
- Synchronization and incremental transfers
- Advanced patterns and automation

### Episode 6: Temporary Storage in Jobs (50 min + 20 min exercises)
Optimize job performance using temporary storage:
- Understanding scratch and tmpfs
- Copying data in job scripts
- Performance benefits (4-20x faster)
- Implementing safe data transfer patterns

### Episode 7: Best Practices (45 min + 15 min exercises)
Implement professional data management strategies:
- Choosing transfer methods
- Organizing project data
- Backup and archival strategies
- Data lifecycle management

## Quick Start

### For Participants

1. **Before the Workshop:**
   - Complete [setup.md](setup.md) to verify access
   - Install optional software (FileZilla)
   - Prepare test data files

2. **During the Workshop:**
   - Follow along with hands-on exercises
   - Ask questions freely
   - Try examples on your own system

3. **After the Workshop:**
   - Use [reference.md](reference.md) for quick command lookup
   - Apply best practices to your projects
   - Contact HPC support for advanced help

### For Instructors

1. **Preparation:**
   - Review [instructor-notes.md](instructor-notes.md)
   - Set up demo files on Sagehen
   - Test all command examples
   - Verify OnDemand and FileZilla connectivity

2. **Teaching:**
   - Use provided diagrams and examples
   - Live demonstrations of all tools
   - Guide participants through hands-on exercises
   - Support troubleshooting

3. **Assessment:**
   - Observe whether participants can complete challenges
   - Check understanding of key concepts
   - Gather feedback for future improvements

## Learning Outcomes

After completing this workshop, you will be able to:

1. Describe Sagehen's storage hierarchy and select appropriate locations
2. Monitor storage quotas and manage disk space effectively
3. Transfer files using multiple methods (OnDemand, FileZilla, rsync)
4. Use temporary storage in job scripts for 4-20x performance improvement
5. Implement professional data organization and backup strategies

## Key Features

- **7 comprehensive episodes** covering all aspects of data management
- **Hands-on exercises** with real scenarios
- **Multiple learning modalities** (GUI, command-line, web interface)
- **Real-world examples** from various research fields
- **Complete reference materials** for post-workshop use
- **Instructor guide** with tips and troubleshooting

## Technical Requirements

- Active Sagehen HPC account
- SSH client (built-in on macOS/Linux, WSL on Windows)
- Web browser for OnDemand
- Optional: FileZilla for Episode 4
- Optional: rsync for Episode 5

## Support

- **Before Workshop:** Review setup.md
- **During Workshop:** Ask instructor questions
- **After Workshop:** Use reference.md, contact its-hpc@pomona.edu
- **Advanced Help:** Email its-hpc@pomona.edu

## Files and Structure

```
data-transfer-storage/
├── config.yaml              # Workshop configuration
├── index.md                 # Workshop landing page
├── setup.md                 # Pre-workshop setup guide
├── reference.md             # Quick reference commands
├── instructor-notes.md      # Teaching guide
├── learner-profiles.md      # Example participant profiles
├── links.md                 # External resources
├── README.md                # This file
└── episodes/
    ├── 01-storage-hierarchy.md
    ├── 02-quotas-management.md
    ├── 03-ondemand-transfers.md
    ├── 04-sftp-filezilla.md
    ├── 05-rsync-cli.md
    ├── 06-scratch-tmpfs.md
    └── 07-best-practices.md
```

## Citation

If you use this workshop, please cite:

```
Pomona College HPC Training Team. (2026). Data Transfer and Storage Management
on Sagehen HPC Cluster. Workshop 12. Available at
https://github.com/pomona-college-hpc/data-transfer-storage
```

## License

This work is licensed under Creative Commons Attribution 4.0 International (CC-BY 4.0).

You are free to:
- Share and redistribute this material
- Adapt, transform, and build upon it

With the requirement that you attribute the original authors.

See [LICENSE.md](LICENSE.md) for full terms.

## Acknowledgments

- Developed for Pomona College HPC Training Program
- Sagehen HPC cluster managed by Pomona College IT
- Based on The Carpentries teaching methodology
- Tested with real Sagehen users and researchers

## Contact

- **HPC Support:** its-hpc@pomona.edu
- **HPC Coordinator:** Andrew Wilson
- **IT Help Desk:** its-help@pomona.edu
- **GitHub:** https://github.com/pomona-college-hpc/data-transfer-storage

## Version Information

- **Created:** March 5, 2026
- **Version:** 1.0
- **Last Updated:** March 5, 2026
- **Status:** Pre-alpha (actively used and refined)

## Contributing

Suggestions for improvements welcome! Contact its-hpc@pomona.edu with:
- Feedback on content clarity
- Additional examples needed
- Topics that need expansion
- Bug reports or corrections

## Related Training

This workshop complements:
- Linux command-line training (prerequisite)
- Git version control (for code management)
- SLURM job submission (for running jobs)
- Python scripting (for workflow automation)

---

**Ready to get started?** Begin with [setup.md](setup.md), then visit [index.md](index.md) for the workshop content.

Questions? Contact its-hpc@pomona.edu

<!-- highlight <labname>/<myusername> placeholders in code blocks; remove if the varnish theme handles this natively -->
<script>(function(){var CSS='.sh-placeholder{color:#c2410c;font-weight:700}[data-bs-theme="dark"] .sh-placeholder,html.dark .sh-placeholder{color:#fdba74}@media (prefers-color-scheme: dark){[data-bs-theme="auto"] .sh-placeholder{color:#fdba74}}';var RX=/<labname>|<myusername>/g;function firstMatch(el){var w=document.createTreeWalker(el,NodeFilter.SHOW_TEXT,null),nodes=[],full='';while(w.nextNode()){nodes.push({n:w.currentNode,s:full.length});full+=w.currentNode.nodeValue;}RX.lastIndex=0;var m;while((m=RX.exec(full))){var s=m.index,e=s+m[0].length,inSpan=false,parts=[];for(var j=0;j<nodes.length;j++){var ns=nodes[j].s,ne=ns+nodes[j].n.nodeValue.length;if(ne<=s||ns>=e)continue;parts.push({node:nodes[j].n,a:Math.max(s-ns,0),b:Math.min(e-ns,nodes[j].n.nodeValue.length)});var p=nodes[j].n.parentNode;while(p&&p!==el){if(p.classList&&p.classList.contains('sh-placeholder')){inSpan=true;break;}p=p.parentNode;}}if(!inSpan&&parts.length)return parts;}return null;}function wrapParts(parts){for(var i=parts.length-1;i>=0;i--){var t=parts[i].node,txt=t.nodeValue,a=parts[i].a,b=parts[i].b;var span=document.createElement('span');span.className='sh-placeholder';span.textContent=txt.slice(a,b);var f=document.createDocumentFragment();if(a>0)f.appendChild(document.createTextNode(txt.slice(0,a)));f.appendChild(span);if(b<txt.length)f.appendChild(document.createTextNode(txt.slice(b)));t.parentNode.replaceChild(f,t);}}function run(){var st=document.createElement('style');st.textContent=CSS;document.head.appendChild(st);document.querySelectorAll('pre,code').forEach(function(el){var guard=0,parts;while((parts=firstMatch(el))&&guard++<500){wrapParts(parts);}});}if(document.readyState==='loading'){document.addEventListener('DOMContentLoaded',run);}else{run();}})();</script>
