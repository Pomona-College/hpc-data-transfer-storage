---
title: "Troubleshooting and Review"
teaching: 15
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- What are the most common data transfer and storage problems?
- How do I diagnose and fix connection issues?
- What checklist should I follow for data management?

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

After completing this episode, participants will be able to:
- Diagnose common transfer and storage errors
- Apply systematic troubleshooting steps
- Use the data management checklist for ongoing work

::::::::::::::::::::::::::::::::::::::::::::::::::

## Common Transfer Issues

### Upload Fails or Hangs (OnDemand)

1. Check file size -- must be under ~2 GB
2. Try a smaller file to test connectivity
3. Refresh browser and retry
4. Switch to rsync for large files

### Connection Refused (FileZilla)

1. Verify hostname: `sagehen.hpc.pomona.edu`
2. Verify port: 22
3. Check internet connection
4. Test with: `ping sagehen.hpc.pomona.edu`

### Authentication Failed

1. Verify Pomona credentials (not a separate Sagehen password)
2. Check caps lock
3. Look for Duo notification on your phone
4. Try SSH first to isolate the issue: `ssh <myusername>@sagehen.hpc.pomona.edu`

### rsync Transfer Stalls

1. Check network connection
2. If interrupted, re-run the same `rsync -avhP` command to resume
3. Try at a different time (off-peak may be faster)
4. For very slow connections, add `-z` for compression

## Common Storage Issues

### Disk Quota Exceeded

Run `quota_check.sh` to see combined lab usage. Even if your home looks fine, another lab member may have filled the shared 1TB quota. Clean up with:

```bash
# Find large files
du --apparent-size -h /rhome/<myusername> | sort -h | tail -10

# Check hidden directories
du --apparent-size -h /rhome/<myusername>/.cache
du --apparent-size -h /rhome/<myusername>/.local

# Archive old projects
tar czf /bigdata/lab/<labname>/archive/old_project.tar.gz /rhome/<myusername>/old_project/
rm -rf /rhome/<myusername>/old_project/
```

### Results Lost from Temporary Storage

If results were in `/scratch` or `/tmpfs` and the job has completed, the data is gone. Prevention: always include a copy-back step in your job scripts before the job ends.

### du Shows Different Size Than quota_check.sh

Use `du --apparent-size` on BeeGFS. Without this flag, `du` reports block usage which differs from actual file size.

### File Permission Denied on Shared Data

Set correct permissions for lab-shared files:
```bash
chmod 644 /bigdata/lab/<labname>/shared_file.txt
chmod 755 /bigdata/lab/<labname>/shared_dir/
```

## Data Management Checklist

::::::::::::::::::::::::::::::::::::: callout

### Ongoing Data Management

**Monthly**
- Run `quota_check.sh` and review usage
- Delete or archive old files

**Before large jobs**
- Verify available quota
- Plan scratch/tmpfs usage in job script
- Include copy-back step for results

**After projects**
- Archive completed projects to `/bigdata`
- Back up results locally or to git/cloud
- Remove intermediate files

**For transfers**
- Use rsync with `-P` for large files
- Verify file sizes after transfer
- Compress data before transferring when possible

::::::::::::::::::::::::::::::::::::::::::::::::::

## Quick Reference

```bash
# Check quota
quota_check.sh

# Accurate directory size on BeeGFS
du --apparent-size -sh /rhome/<myusername>

# Upload with rsync
rsync -avhP file.dat <myusername>@sagehen.hpc.pomona.edu:/rhome/<myusername>/

# Download with rsync
rsync -avhP <myusername>@sagehen.hpc.pomona.edu:/rhome/<myusername>/file.dat ~/

# Sync directory
rsync -avhPr ~/mydir/ <myusername>@sagehen.hpc.pomona.edu:/rhome/<myusername>/mydir/

# Archive project
tar czf project.tar.gz project/

# Contact for help
# its-hpc@pomona.edu
```

::::::::::::::::::::::::::::::::::::: challenge

## Challenge: Troubleshooting Scenarios

For each scenario, identify the problem and the solution:

1. You run `cp output.dat /rhome/<myusername>/` and get "Disk quota exceeded", but `du -sh /rhome/<myusername>` shows only 200 GB used
2. Your job script writes results to `/tmpfs` but when you check after the job, the files are gone
3. You uploaded a 500 MB file via OnDemand but it does not appear in the file listing

::::::::::::::::::::::::::::::::::::: solution

1. **Problem**: Using `du` without `--apparent-size` gives inaccurate sizes on BeeGFS, or the combined lab quota (including other members and `/bigdata`) is full. **Solution**: Run `quota_check.sh` for the true combined usage. Coordinate with lab members to free space.

2. **Problem**: `/tmpfs` is deleted when the job completes. **Solution**: Add `rsync -avhP $TMPFS/output.dat /rhome/<myusername>/results/` before the job ends. Always copy results from temporary storage to permanent storage.

3. **Problem**: Browser cache showing stale listing, or upload failed silently. **Solution**: Press F5 to refresh. If still missing, check the upload log, verify quota is not exceeded, and try uploading again. For files over 500 MB, use rsync instead.

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Most transfer issues stem from network problems, authentication, or file size limits
- Most storage issues stem from quota exhaustion or incorrect `du` usage on BeeGFS
- Always use `quota_check.sh` for accurate quota information
- Temporary storage is deleted when jobs complete -- always copy results back
- Contact its-hpc@pomona.edu for issues you cannot resolve

::::::::::::::::::::::::::::::::::::::::::::::::::

<!-- highlight <labname>/<myusername> placeholders in code blocks; remove if the varnish theme handles this natively -->
<script>(function(){var CSS='.sh-placeholder{color:#c2410c;font-weight:700}[data-bs-theme="dark"] .sh-placeholder,html.dark .sh-placeholder{color:#fdba74}@media (prefers-color-scheme: dark){[data-bs-theme="auto"] .sh-placeholder{color:#fdba74}}';var RX=/<labname>|<myusername>/g;function firstMatch(el){var w=document.createTreeWalker(el,NodeFilter.SHOW_TEXT,null),nodes=[],full='';while(w.nextNode()){nodes.push({n:w.currentNode,s:full.length});full+=w.currentNode.nodeValue;}RX.lastIndex=0;var m;while((m=RX.exec(full))){var s=m.index,e=s+m[0].length,inSpan=false,parts=[];for(var j=0;j<nodes.length;j++){var ns=nodes[j].s,ne=ns+nodes[j].n.nodeValue.length;if(ne<=s||ns>=e)continue;parts.push({node:nodes[j].n,a:Math.max(s-ns,0),b:Math.min(e-ns,nodes[j].n.nodeValue.length)});var p=nodes[j].n.parentNode;while(p&&p!==el){if(p.classList&&p.classList.contains('sh-placeholder')){inSpan=true;break;}p=p.parentNode;}}if(!inSpan&&parts.length)return parts;}return null;}function wrapParts(parts){for(var i=parts.length-1;i>=0;i--){var t=parts[i].node,txt=t.nodeValue,a=parts[i].a,b=parts[i].b;var span=document.createElement('span');span.className='sh-placeholder';span.textContent=txt.slice(a,b);var f=document.createDocumentFragment();if(a>0)f.appendChild(document.createTextNode(txt.slice(0,a)));f.appendChild(span);if(b<txt.length)f.appendChild(document.createTextNode(txt.slice(b)));t.parentNode.replaceChild(f,t);}}function run(){var st=document.createElement('style');st.textContent=CSS;document.head.appendChild(st);document.querySelectorAll('pre,code').forEach(function(el){var guard=0,parts;while((parts=firstMatch(el))&&guard++<500){wrapParts(parts);}});}if(document.readyState==='loading'){document.addEventListener('DOMContentLoaded',run);}else{run();}})();</script>
