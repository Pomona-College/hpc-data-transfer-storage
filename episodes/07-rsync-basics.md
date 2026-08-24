---
title: "Introduction to rsync"
teaching: 25
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- What is rsync and why is it preferred for large file transfers?
- How do I upload and download files with rsync?
- What do the common rsync flags mean?

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

After completing this episode, participants will be able to:

- Explain rsync's advantages over other transfer methods
- Use rsync to upload files to Sagehen
- Use rsync to download files from Sagehen
- Choose appropriate flags for different transfer scenarios

::::::::::::::::::::::::::::::::::::::::::::::::::

## Why rsync?

**rsync** (remote synchronization) is a command-line tool designed for efficient, reliable file transfer. Key advantages:

1. **Incremental**: Only transfers changed data on subsequent runs
2. **Resumable**: Can resume interrupted transfers with `-P` flag
3. **Efficient**: Compresses data during transfer with `-z` flag
4. **Scriptable**: Ideal for automated and batch operations

rsync comes pre-installed on macOS and Linux.

::::::::::::::::::::::::::::::::::::::: callout

## Windows Users: Git Bash Does NOT Include rsync

If you've been following along in Git Bash, `rsync` will fail with
`command not found` -- Git Bash simply doesn't ship it, and rsync must be
installed on *both* ends of a transfer. Your options:

1. **Use WSL** (Windows Subsystem for Linux): install it once with
   `wsl --install` in an Administrator PowerShell; every command in this
   episode then works unchanged inside WSL.
2. **Skip rsync locally** and use `scp` or FileZilla (earlier episodes) for
   Windows-to-Sagehen transfers.
3. rsync still works **on Sagehen itself** (e.g. copying between `/rhome` and
   `/bigdata` in an SSH session), so the server-side examples below work for
   everyone.

:::::::::::::::::::::::::::::::::::::::::::::::

## Basic Syntax

**Upload to Sagehen:**
```bash
rsync -avhP local_file <myusername>@sagehen.hpc.pomona.edu:/rhome/<myusername>/destination/
```

**Download from Sagehen:**
```bash
rsync -avhP <myusername>@sagehen.hpc.pomona.edu:/rhome/<myusername>/source_file local_destination/
```

## Essential Flags

| Flag | Meaning | When to Use |
|------|---------|-------------|
| `-a` | Archive mode (preserves permissions, timestamps) | Always |
| `-v` | Verbose output | Debugging |
| `-h` | Human-readable sizes (MB, GB) | Always |
| `-P` | Show progress + keep partial files for resume | Large files |
| `-r` | Recursive (directory contents) | Directories |
| `-z` | Compress during transfer | Slow networks |

::::::::::::::::::::::::::::::::::::: callout

## Trailing Slashes Matter

```bash
# WITH trailing slash: copies CONTENTS of myproject into /dest/
rsync -avhP ~/myproject/ user@host:/dest/

# WITHOUT trailing slash: copies the DIRECTORY itself, creating /dest/myproject/
rsync -avhP ~/myproject user@host:/dest/
```

This is the most common rsync mistake. Use trailing slashes when you want to transfer directory **contents**.

::::::::::::::::::::::::::::::::::::::::::::::::::

## Uploading a Single File

```bash
rsync -avhP ~/mydata.csv <myusername>@sagehen.hpc.pomona.edu:/rhome/<myusername>/data/
```

Output:
```
mydata.csv
    50,000,000 100%   15.23MB/s    0:00:03 (xfr#1, to-chk=0/1)

sent 50,001,234 bytes  received 35 bytes  13,333,664 bytes/sec
total size is 50,000,000  speedup is 1.00
```

## Uploading a Directory

```bash
rsync -avhPr ~/myproject/ <myusername>@sagehen.hpc.pomona.edu:/rhome/<myusername>/projects/myproject/
```

All files and subdirectories are transferred recursively.

## Downloading Files

```bash
rsync -avhP <myusername>@sagehen.hpc.pomona.edu:/rhome/<myusername>/results/output.h5 ~/Downloads/
```

## Resuming Interrupted Transfers

The `-P` flag is essential. If a transfer is interrupted, simply run the same command again:

```bash
# Original command (interrupted by network drop)
rsync -avhP ~/large_file.tar.gz <myusername>@sagehen.hpc.pomona.edu:/rhome/<myusername>/

# Resume by running the exact same command
rsync -avhP ~/large_file.tar.gz <myusername>@sagehen.hpc.pomona.edu:/rhome/<myusername>/
```

rsync detects the existing partial file and resumes from where it stopped.

## Incremental Synchronization

On the second run, rsync only transfers new or changed files:

```bash
# First run: transfers all files
rsync -avhPr ~/myproject/ <myusername>@sagehen.hpc.pomona.edu:/rhome/<myusername>/myproject/

# Add a new file locally, then run again
rsync -avhPr ~/myproject/ <myusername>@sagehen.hpc.pomona.edu:/rhome/<myusername>/myproject/
# Only the new file transfers; unchanged files are skipped
```

::::::::::::::::::::::::::::::::::::: challenge

## Challenge: First rsync Transfer

1. Create a 10 MB test file:
   ```bash
   dd if=/dev/zero of=test_data.bin bs=1M count=10
   ```
2. Upload to Sagehen:
   ```bash
   rsync -avhP test_data.bin <myusername>@sagehen.hpc.pomona.edu:/rhome/<myusername>/
   ```
3. Verify on Sagehen:
   ```bash
   ls -lh /rhome/<myusername>/test_data.bin
   ```

What was your transfer speed? Does the file size match?

::::::::::::::::::::::::::::::::::::: solution

Expected output:

```
test_data.bin
    10,485,760 100%    8.34MB/s    0:00:01 (xfr#1, to-chk=0/1)
```

On Sagehen, `ls -lh` should show 10M. The file transferred completely.

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- rsync is the fastest and most reliable method for large file transfers
- Always use `-P` for large files to enable resume capability
- The trailing slash on source paths is critical: `source/` vs `source`
- rsync automatically skips unchanged files on subsequent runs
- Use `-avhP` as your standard flag combination

::::::::::::::::::::::::::::::::::::::::::::::::::

<!-- highlight <labname>/<myusername> placeholders in code blocks; remove if the varnish theme handles this natively -->
<script>(function(){var CSS='.sh-placeholder{color:#c2410c;font-weight:700}[data-bs-theme="dark"] .sh-placeholder,html.dark .sh-placeholder{color:#fdba74}@media (prefers-color-scheme: dark){[data-bs-theme="auto"] .sh-placeholder{color:#fdba74}}';var RX=/<labname>|<myusername>/g;function firstMatch(el){var w=document.createTreeWalker(el,NodeFilter.SHOW_TEXT,null),nodes=[],full='';while(w.nextNode()){nodes.push({n:w.currentNode,s:full.length});full+=w.currentNode.nodeValue;}RX.lastIndex=0;var m;while((m=RX.exec(full))){var s=m.index,e=s+m[0].length,inSpan=false,parts=[];for(var j=0;j<nodes.length;j++){var ns=nodes[j].s,ne=ns+nodes[j].n.nodeValue.length;if(ne<=s||ns>=e)continue;parts.push({node:nodes[j].n,a:Math.max(s-ns,0),b:Math.min(e-ns,nodes[j].n.nodeValue.length)});var p=nodes[j].n.parentNode;while(p&&p!==el){if(p.classList&&p.classList.contains('sh-placeholder')){inSpan=true;break;}p=p.parentNode;}}if(!inSpan&&parts.length)return parts;}return null;}function wrapParts(parts){for(var i=parts.length-1;i>=0;i--){var t=parts[i].node,txt=t.nodeValue,a=parts[i].a,b=parts[i].b;var span=document.createElement('span');span.className='sh-placeholder';span.textContent=txt.slice(a,b);var f=document.createDocumentFragment();if(a>0)f.appendChild(document.createTextNode(txt.slice(0,a)));f.appendChild(span);if(b<txt.length)f.appendChild(document.createTextNode(txt.slice(b)));t.parentNode.replaceChild(f,t);}}function run(){var st=document.createElement('style');st.textContent=CSS;document.head.appendChild(st);document.querySelectorAll('pre,code').forEach(function(el){var guard=0,parts;while((parts=firstMatch(el))&&guard++<500){wrapParts(parts);}});}if(document.readyState==='loading'){document.addEventListener('DOMContentLoaded',run);}else{run();}})();</script>
