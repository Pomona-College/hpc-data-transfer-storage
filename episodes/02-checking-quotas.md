---
title: "Checking and Managing Quotas"
teaching: 20
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- How much storage can I use on Sagehen?
- How do I check my current storage usage?
- Why does `du` give inaccurate results on BeeGFS?

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

After completing this episode, participants will be able to:
- Use `quota_check.sh` to monitor storage usage
- Understand the difference between apparent size and block size on BeeGFS
- Use `du` with the correct flags for accurate measurements
- Identify large files and directories consuming space

::::::::::::::::::::::::::::::::::::::::::::::::::

## Quota Structure on Sagehen

Both `/rhome/<myusername>` and `/bigdata/lab/<labname>` share a **lab-based quota**:

- **Quota per lab**: 1TB combined across home + lab storage
- **Shared responsibility**: All lab members share this quota
- **Enforcement**: Hard limit -- cannot write new files if exceeded
- **Exceptions**: Request temporary increase by contacting its-hpc@pomona.edu

```
Lab ABC (quota: 1TB = 1024GB)
+-- /rhome/alice       (150 GB)
+-- /rhome/bob         (120 GB)
+-- /rhome/charlie     (180 GB)
+-- /bigdata/lab/abc       (574 GB)
    Total Used: 1024 GB (at limit!)
```

Temporary storage (`/scratch` and `/tmpfs`) does not count against your lab quota.

## Using quota_check.sh (Recommended)

Sagehen provides a dedicated script that accounts for BeeGFS:

```bash
quota_check.sh
```

Real output — a BeeGFS quota table with a GROUP row (your lab's shared pool) and a User row:

![`quota_check.sh` reports the lab's group usage against the shared pool limit, then your per-user usage. The group limit is what's enforced.](fig/02-quota-check-output.png){alt='Terminal output of quota_check.sh on Sagehen. A GROUP Quota section shows the lab group using 508.87 GiB of a 931.32 GiB limit with 1.16 million inodes and no inode limit. A User Quota section shows the individual user consuming 1.22 GiB with no per-user space limit shown. Both rows reference storage_pool_default.'}

Note the enforcement model: the *group* (lab) row carries the hard limit on the shared pool; the user row typically shows no separate hard cap.

## Using du Correctly on BeeGFS

The `du` command requires `--apparent-size` for accuracy on BeeGFS:

```bash
# Inaccurate (reports block usage)
du -sh /rhome/<myusername>
  Result: 156 GB

# Accurate (reports actual data size)
du -sh --apparent-size /rhome/<myusername>
  Result: 150 GB
```

**Always use** `du --apparent-size` on Sagehen.

## Finding Large Files and Directories

**Largest directories:**
```bash
du --apparent-size -h /rhome/<myusername> | sort -h
```

**Largest individual files:**
```bash
find /rhome/<myusername> -type f -printf '%s %p\n' | sort -rn | head -20
```

## What Happens When You Exceed Quota

At 100% quota, you **cannot write new files**:

```bash
$ cp large_file.dat /rhome/<myusername>/
cp: cannot create regular file: Disk quota exceeded
```

You can still read files, list directories, and run jobs that only read from disk. But you cannot write, modify, or save job outputs.

To request a quota increase, contact its-hpc@pomona.edu with your lab name, justification, specific data size needed, and cleanup timeline.

::::::::::::::::::::::::::::::::::::: challenge

## Challenge: Check Your Quota

1. Run `quota_check.sh` and record your current usage
2. Find your 5 largest directories:
   ```bash
   du --apparent-size -h /rhome/<myusername> | sort -h | tail -5
   ```
3. Compare your home directory usage with lab storage

Are you above or below 80% usage? Which is larger: your home directory or lab storage?

::::::::::::::::::::::::::::::::::::: solution

Sample output:
```bash
$ quota_check.sh
Combined Usage:
  Total Used:   724.5 GB
  Total Quota:  1024 GB
  Percent:      70.7%
Status: OK (below quota)
```

Your specific numbers will vary. If above 80%, consider cleanup strategies covered in the next episode.

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Use `quota_check.sh` for official quota monitoring on Sagehen
- Always use `du --apparent-size` on BeeGFS for accurate size measurements
- Your lab shares a 1TB quota across all members and both `/rhome` and `/bigdata`
- You cannot write files if quota is exceeded -- monitor regularly
- Temporary storage (`/scratch`, `/tmpfs`) does not count against quota

::::::::::::::::::::::::::::::::::::::::::::::::::

<!-- highlight <labname>/<myusername> placeholders in code blocks; remove if the varnish theme handles this natively -->
<script>(function(){var CSS='.sh-placeholder{color:#c2410c;font-weight:700}[data-bs-theme="dark"] .sh-placeholder,html.dark .sh-placeholder{color:#fdba74}@media (prefers-color-scheme: dark){[data-bs-theme="auto"] .sh-placeholder{color:#fdba74}}';var RX=/<labname>|<myusername>/g;function firstMatch(el){var w=document.createTreeWalker(el,NodeFilter.SHOW_TEXT,null),nodes=[],full='';while(w.nextNode()){nodes.push({n:w.currentNode,s:full.length});full+=w.currentNode.nodeValue;}RX.lastIndex=0;var m;while((m=RX.exec(full))){var s=m.index,e=s+m[0].length,inSpan=false,parts=[];for(var j=0;j<nodes.length;j++){var ns=nodes[j].s,ne=ns+nodes[j].n.nodeValue.length;if(ne<=s||ns>=e)continue;parts.push({node:nodes[j].n,a:Math.max(s-ns,0),b:Math.min(e-ns,nodes[j].n.nodeValue.length)});var p=nodes[j].n.parentNode;while(p&&p!==el){if(p.classList&&p.classList.contains('sh-placeholder')){inSpan=true;break;}p=p.parentNode;}}if(!inSpan&&parts.length)return parts;}return null;}function wrapParts(parts){for(var i=parts.length-1;i>=0;i--){var t=parts[i].node,txt=t.nodeValue,a=parts[i].a,b=parts[i].b;var span=document.createElement('span');span.className='sh-placeholder';span.textContent=txt.slice(a,b);var f=document.createDocumentFragment();if(a>0)f.appendChild(document.createTextNode(txt.slice(0,a)));f.appendChild(span);if(b<txt.length)f.appendChild(document.createTextNode(txt.slice(b)));t.parentNode.replaceChild(f,t);}}function run(){var st=document.createElement('style');st.textContent=CSS;document.head.appendChild(st);document.querySelectorAll('pre,code').forEach(function(el){var guard=0,parts;while((parts=firstMatch(el))&&guard++<500){wrapParts(parts);}});}if(document.readyState==='loading'){document.addEventListener('DOMContentLoaded',run);}else{run();}})();</script>
