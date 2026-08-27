---
site: sandpaper::sandpaper_site
---

# Data Transfer and Storage Management on Sagehen

Welcome to Workshop 12: Data Transfer and Storage Management for Pomona College's Sagehen HPC cluster.

## Overview

In this workshop, you will learn how to:

- Navigate Sagehen's hierarchical storage system
- Understand storage quotas and monitor your disk usage
- Transfer files efficiently using multiple methods
- Use temporary storage for high-performance computing jobs
- Choose the right tool for different transfer scenarios
- Implement best practices for data organization and backup

## Target Audience

This workshop is designed for researchers and students who need to:

- Work with data on the Sagehen HPC cluster
- Transfer files to and from Sagehen
- Manage their storage allocation effectively
- Optimize file transfer performance

## Learning Outcomes

After completing this workshop, you will be able to:

1. Describe the Sagehen storage hierarchy and when to use each filesystem
2. Check your storage quotas and manage disk space
3. Transfer files using the web interface, SFTP, and rsync
4. Copy data to/from temporary storage in job scripts
5. Select appropriate transfer methods for different scenarios
6. Implement data organization and backup strategies

## Prerequisites

- Active Sagehen account with OnDemand access
- Basic familiarity with Linux/Unix command line (helpful for rsync episodes)
- SSH client software (built-in on macOS/Linux, use Windows Subsystem for Linux on Windows)
- FileZilla installed (optional, for SFTP episodes)

## Cluster Information

- **Cluster Name**: Sagehen
- **File System**: BeeGFS (parallel distributed filesystem)
- **Access**: OnDemand web interface or SSH
- **Support Email**: its-hpc@pomona.edu
- **Support Contact**: Andrew Wilson

## Quick Reference

### Storage Locations

| Path | Purpose | Quota | Persistence |
|------|---------|-------|-------------|
| `/rhome/<myusername>` | Home directory | Lab quota | Permanent |
| `/bigdata/lab/<labname>` | Shared lab storage | 1TB per lab | Permanent |
| `/scratch/$SLURM_JOB_ID` | Job-local SSD temp | Job time limit | Deleted after job |
| `/tmpfs/$SLURM_JOB_ID` | RAM-backed temp | Job memory | Deleted after job |

### Transfer Methods

| Method | Use Case | Speed | Graphical |
|--------|----------|-------|-----------|
| OnDemand Web UI | Small files (<100 MB) | Slow | Yes |
| FileZilla SFTP | Interactive transfer | Medium | Yes |
| rsync | Large files, sync | Fast | No |
| rclone | Cloud storage | Medium | Via OnDemand |

## Workshop Structure

This workshop consists of 14 episodes covering progressively more advanced topics:

1. **Storage Hierarchy** - Understand the filesystem layout
2. **Quotas Management** - Monitor and manage your storage
3. **OnDemand Transfers** - Use the web interface
4. **SFTP with FileZilla** - Graphical file transfers
5. **rsync CLI** - Command-line bulk transfers
6. **Scratch and Tmpfs** - Temporary storage in jobs
7. **Best Practices** - Storage strategy and organization

Estimated total time: 4-5 hours

## Getting Help

- **Before Workshop**: Review the setup instructions in the [Setup](setup.md) page
- **During Workshop**: Instructors will guide you through each episode
- **After Workshop**: Refer to the [Reference](reference.md) page for command summaries
- **Technical Support**: Contact its-hpc@pomona.edu

---

**Last Updated**: March 5, 2026
**License**: CC-BY 4.0
**Carpentry**: The Carpentries Incubator

<!-- highlight <labname>/<myusername> placeholders in code blocks; remove if the varnish theme handles this natively -->
<script>(function(){var CSS='.sh-placeholder{color:#c2410c;font-weight:700}[data-bs-theme="dark"] .sh-placeholder,html.dark .sh-placeholder{color:#fdba74}@media (prefers-color-scheme: dark){[data-bs-theme="auto"] .sh-placeholder{color:#fdba74}}';var RX=/<labname>|<myusername>/g;function firstMatch(el){var w=document.createTreeWalker(el,NodeFilter.SHOW_TEXT,null),nodes=[],full='';while(w.nextNode()){nodes.push({n:w.currentNode,s:full.length});full+=w.currentNode.nodeValue;}RX.lastIndex=0;var m;while((m=RX.exec(full))){var s=m.index,e=s+m[0].length,inSpan=false,parts=[];for(var j=0;j<nodes.length;j++){var ns=nodes[j].s,ne=ns+nodes[j].n.nodeValue.length;if(ne<=s||ns>=e)continue;parts.push({node:nodes[j].n,a:Math.max(s-ns,0),b:Math.min(e-ns,nodes[j].n.nodeValue.length)});var p=nodes[j].n.parentNode;while(p&&p!==el){if(p.classList&&p.classList.contains('sh-placeholder')){inSpan=true;break;}p=p.parentNode;}}if(!inSpan&&parts.length)return parts;}return null;}function wrapParts(parts){for(var i=parts.length-1;i>=0;i--){var t=parts[i].node,txt=t.nodeValue,a=parts[i].a,b=parts[i].b;var span=document.createElement('span');span.className='sh-placeholder';span.textContent=txt.slice(a,b);var f=document.createDocumentFragment();if(a>0)f.appendChild(document.createTextNode(txt.slice(0,a)));f.appendChild(span);if(b<txt.length)f.appendChild(document.createTextNode(txt.slice(b)));t.parentNode.replaceChild(f,t);}}function run(){var st=document.createElement('style');st.textContent=CSS;document.head.appendChild(st);document.querySelectorAll('pre,code').forEach(function(el){var guard=0,parts;while((parts=firstMatch(el))&&guard++<500){wrapParts(parts);}});}if(document.readyState==='loading'){document.addEventListener('DOMContentLoaded',run);}else{run();}})();</script>

## Acknowledgments

Developed by **Andrew Wilson**, Director of Research Computing and Digital
Scholarship at Pomona College, with **Andrei Motchenko**, who tested, edited
and produced screenshots for the workshop series.
