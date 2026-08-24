---
title: "Quota Cleanup Strategies"
teaching: 15
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- How do I free up space when approaching my quota?
- What are effective strategies for reducing storage usage?
- How do I troubleshoot common quota issues?

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

After completing this episode, participants will be able to:
- Implement regular cleanup routines
- Use compression to reduce storage usage
- Identify and remove redundant files
- Troubleshoot common quota problems

::::::::::::::::::::::::::::::::::::::::::::::::::

## Regular Cleanup

Run a monthly maintenance routine:

```bash
# Check quota
quota_check.sh

# Find old files (not accessed in 90 days)
find /rhome/<myusername> -type f -atime +90 -exec ls -lh {} \;

# Archive and remove old project
tar czf /bigdata/lab/<labname>/archive_2025_03.tar.gz /rhome/<myusername>/old_project/
rm -rf /rhome/<myusername>/old_project/
```

## Compression

Compress old data to reclaim space:

```bash
tar czf project_archive.tar.gz project_name/

# Check compression ratio
du --apparent-size -h project_archive.tar.gz
du --apparent-size -h project_name/
```

Typical compression for text/CSV data: 5-20x smaller.

## Identifying Redundancy

Remove backup and temporary files:

```bash
# Find backup files
find /rhome/<myusername> -name "*.bak" -o -name "*_old" -o -name "*~"

# Remove them
find /rhome/<myusername> -name "*.bak" -delete
```

Avoid storing multiple manual copies of the same project. Use git version control instead:

```bash
cd /rhome/<myusername>/project
git init
git add .
git commit -m "Initial version"
```

## Monitoring and Alerts

Your lab receives email notifications at 80%, 95%, and 100% quota usage. You can also monitor quota during file transfers:

```bash
watch -n 5 quota_check.sh
```

::::::::::::::::::::::::::::::::::::: callout

### Quota Management Checklist

- **Monthly**: Run `quota_check.sh` and review usage
- **Quarterly**: Clean up files older than 6 months
- **After projects**: Archive and remove old project directories
- **Before large jobs**: Verify available quota
- **For temporary work**: Use `/scratch` (does not count against quota)

::::::::::::::::::::::::::::::::::::::::::::::::::

## Troubleshooting Common Issues

### "Disk quota exceeded" but du shows space

Your lab's combined quota may be full even if your home looks fine. Run `quota_check.sh` to see the combined usage. Also check hidden directories:

```bash
du --apparent-size -h /rhome/<myusername>/.cache
du --apparent-size -h /rhome/<myusername>/.local
```

### du reports different size than quota_check.sh

You likely forgot the `--apparent-size` flag. Without it, `du` reports block usage on BeeGFS, which differs from actual data size.

### Deleted files but usage unchanged

A running process may still hold the file handle open:

```bash
lsof /rhome/<myusername>/largefile.dat
kill <process_id>
```

::::::::::::::::::::::::::::::::::::: challenge

## Challenge: Calculate Storage Efficiency

1. Pick a directory and measure its size:
   ```bash
   du --apparent-size -sh /rhome/<myusername>/project1
   ```
2. Calculate what portion of your 1TB quota it uses
3. Identify at least one file or directory you could safely delete or compress

::::::::::::::::::::::::::::::::::::: solution

Example:
```
Project directory: 10 GB
Current quota: 1024 GB
Percentage used: 10/1024 = 0.98%
```

Actions to consider: delete `.bak` files, compress older project directories, move shared data to `/bigdata`, remove duplicate datasets.

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Regular cleanup prevents quota-related disruptions
- Compression can reduce text-based data by 5-20x
- Use git instead of manual versioning to avoid duplicate copies
- Always use `du --apparent-size` to get accurate sizes on BeeGFS
- Hidden directories like `.cache` and `.local` can consume significant space

::::::::::::::::::::::::::::::::::::::::::::::::::

<!-- highlight <labname>/<myusername> placeholders in code blocks; remove if the varnish theme handles this natively -->
<script>(function(){var CSS='.sh-placeholder{color:#c2410c;font-weight:700}[data-bs-theme="dark"] .sh-placeholder,html.dark .sh-placeholder{color:#fdba74}@media (prefers-color-scheme: dark){[data-bs-theme="auto"] .sh-placeholder{color:#fdba74}}';var RX=/<labname>|<myusername>/g;function firstMatch(el){var w=document.createTreeWalker(el,NodeFilter.SHOW_TEXT,null),nodes=[],full='';while(w.nextNode()){nodes.push({n:w.currentNode,s:full.length});full+=w.currentNode.nodeValue;}RX.lastIndex=0;var m;while((m=RX.exec(full))){var s=m.index,e=s+m[0].length,inSpan=false,parts=[];for(var j=0;j<nodes.length;j++){var ns=nodes[j].s,ne=ns+nodes[j].n.nodeValue.length;if(ne<=s||ns>=e)continue;parts.push({node:nodes[j].n,a:Math.max(s-ns,0),b:Math.min(e-ns,nodes[j].n.nodeValue.length)});var p=nodes[j].n.parentNode;while(p&&p!==el){if(p.classList&&p.classList.contains('sh-placeholder')){inSpan=true;break;}p=p.parentNode;}}if(!inSpan&&parts.length)return parts;}return null;}function wrapParts(parts){for(var i=parts.length-1;i>=0;i--){var t=parts[i].node,txt=t.nodeValue,a=parts[i].a,b=parts[i].b;var span=document.createElement('span');span.className='sh-placeholder';span.textContent=txt.slice(a,b);var f=document.createDocumentFragment();if(a>0)f.appendChild(document.createTextNode(txt.slice(0,a)));f.appendChild(span);if(b<txt.length)f.appendChild(document.createTextNode(txt.slice(b)));t.parentNode.replaceChild(f,t);}}function run(){var st=document.createElement('style');st.textContent=CSS;document.head.appendChild(st);document.querySelectorAll('pre,code').forEach(function(el){var guard=0,parts;while((parts=firstMatch(el))&&guard++<500){wrapParts(parts);}});}if(document.readyState==='loading'){document.addEventListener('DOMContentLoaded',run);}else{run();}})();</script>
