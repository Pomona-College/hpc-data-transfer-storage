---
title: "Choosing a Transfer Method"
teaching: 15
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- How do I choose the right transfer method for different scenarios?
- What are the tradeoffs between OnDemand, FileZilla, and rsync?

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

After completing this episode, participants will be able to:
- Select the appropriate transfer method based on file size and frequency
- Apply the decision matrix to real research scenarios
- Combine methods effectively in a workflow

::::::::::::::::::::::::::::::::::::::::::::::::::

## Decision Matrix

```
File Size:
  Small (< 50 MB)        --> OnDemand
  Medium (50 MB - 1 GB)  --> FileZilla or rsync
  Large (> 1 GB)         --> rsync

Frequency:
  One-time               --> simplest method
  Occasional (weekly)    --> rsync or FileZilla
  Frequent / automated   --> rsync scripts

Interface:
  Graphical preferred    --> OnDemand or FileZilla
  Command-line preferred --> rsync
```

## Method Comparison

| Method | Speed | GUI | Resume | Batch | Best For |
|--------|-------|-----|--------|-------|----------|
| **OnDemand** | Slow | Yes | No | No | Small files, browsing |
| **FileZilla** | Medium | Yes | Yes | No | Interactive transfers |
| **rsync** | Fast | No | Yes | Yes | Large files, automation |

## Scenario Examples

**Upload 500 MB dataset at project start:**
```bash
rsync -avhP ~/data.tar.gz <myusername>@sagehen.hpc.pomona.edu:/rhome/<myusername>/projects/
```
Method: rsync. Time: ~1 minute.

**Interactive file management during development:**
Use FileZilla for visual feedback and easy drag-and-drop of multiple small files.

**Download 2 GB results file:**
```bash
rsync -avhP <myusername>@sagehen.hpc.pomona.edu:/rhome/<myusername>/results/output.h5 ~/Downloads/
```
Method: rsync. Time: 2-3 minutes.

**Sync code between laptop and Sagehen HPC daily:**
```bash
# Push changes
rsync -avhPr --exclude='.git' ~/myproject/ <myusername>@sagehen.hpc.pomona.edu:/rhome/<myusername>/myproject/
# Pull changes
rsync -avhPr <myusername>@sagehen.hpc.pomona.edu:/rhome/<myusername>/myproject/ ~/myproject/
```

**Weekly backup of entire project:**
```bash
rsync -avhPr --exclude='.git' \
  <myusername>@sagehen.hpc.pomona.edu:/rhome/<myusername>/projects/myproject/ \
  ~/backups/myproject_$(date +%Y%m%d)/
```

::::::::::::::::::::::::::::::::::::: callout

## Compress Before Transferring

Regardless of method, compressing data before transfer saves time:
```bash
tar czf myproject.tar.gz myproject/
```
Text and CSV data compress 5-20x. Already-compressed formats (images, video, archives) do not benefit.

::::::::::::::::::::::::::::::::::::::::::::::::::

## Combining Methods in a Workflow

A typical research workflow might use all three methods:

1. **OnDemand**: Browse files, check results, quick edits
2. **FileZilla**: Interactive upload of new datasets during development
3. **rsync**: Large data transfers, automated backups, job script staging

Choose based on what you are doing at each stage, not one method for everything.

::::::::::::::::::::::::::::::::::::: challenge

## Challenge: Method Selection

For each scenario, choose the best transfer method and explain why:

1. Upload a 10 MB configuration file to Sagehen
2. Transfer a 50 GB genomics dataset to Sagehen
3. Download 200 analysis result files (1-5 MB each) to your laptop
4. Set up a weekly automated backup of your home directory

::::::::::::::::::::::::::::::::::::: solution

1. **OnDemand** -- Small file, one-time, easiest method
2. **rsync** -- Very large file, needs resume capability: `rsync -avhP dataset.tar.gz user@sagehen:/rhome/<myusername>/`
3. **rsync** -- Many files, batch operation: `rsync -avhPr user@sagehen:/rhome/<myusername>/results/ ~/results/`
4. **rsync script via cron** -- Automated, recurring, needs to handle many files efficiently

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Choose transfer method based on file size, frequency, and interface preference
- OnDemand for small files, FileZilla for interactive work, rsync for everything else
- rsync is the only method suitable for automation and very large files
- Compressing data before transfer saves time regardless of method

::::::::::::::::::::::::::::::::::::::::::::::::::

<!-- highlight <labname>/<myusername> placeholders in code blocks; remove if the varnish theme handles this natively -->
<script>(function(){var CSS='.sh-placeholder{color:#c2410c;font-weight:700}[data-bs-theme="dark"] .sh-placeholder,html.dark .sh-placeholder{color:#fdba74}@media (prefers-color-scheme: dark){[data-bs-theme="auto"] .sh-placeholder{color:#fdba74}}';var RX=/<labname>|<myusername>/g;function firstMatch(el){var w=document.createTreeWalker(el,NodeFilter.SHOW_TEXT,null),nodes=[],full='';while(w.nextNode()){nodes.push({n:w.currentNode,s:full.length});full+=w.currentNode.nodeValue;}RX.lastIndex=0;var m;while((m=RX.exec(full))){var s=m.index,e=s+m[0].length,inSpan=false,parts=[];for(var j=0;j<nodes.length;j++){var ns=nodes[j].s,ne=ns+nodes[j].n.nodeValue.length;if(ne<=s||ns>=e)continue;parts.push({node:nodes[j].n,a:Math.max(s-ns,0),b:Math.min(e-ns,nodes[j].n.nodeValue.length)});var p=nodes[j].n.parentNode;while(p&&p!==el){if(p.classList&&p.classList.contains('sh-placeholder')){inSpan=true;break;}p=p.parentNode;}}if(!inSpan&&parts.length)return parts;}return null;}function wrapParts(parts){for(var i=parts.length-1;i>=0;i--){var t=parts[i].node,txt=t.nodeValue,a=parts[i].a,b=parts[i].b;var span=document.createElement('span');span.className='sh-placeholder';span.textContent=txt.slice(a,b);var f=document.createDocumentFragment();if(a>0)f.appendChild(document.createTextNode(txt.slice(0,a)));f.appendChild(span);if(b<txt.length)f.appendChild(document.createTextNode(txt.slice(b)));t.parentNode.replaceChild(f,t);}}function run(){var st=document.createElement('style');st.textContent=CSS;document.head.appendChild(st);document.querySelectorAll('pre,code').forEach(function(el){var guard=0,parts;while((parts=firstMatch(el))&&guard++<500){wrapParts(parts);}});}if(document.readyState==='loading'){document.addEventListener('DOMContentLoaded',run);}else{run();}})();</script>
