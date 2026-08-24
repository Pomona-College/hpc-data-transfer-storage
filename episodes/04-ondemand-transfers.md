---
title: "Transferring Files with OnDemand"
teaching: 20
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- How do I transfer small files without the command line?
- What are the limitations of web-based file transfer?

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

After completing this episode, participants will be able to:

- Access the OnDemand file manager interface
- Upload and download files through the web browser
- Create directories and organize files
- Understand when web transfer is appropriate vs. other methods

::::::::::::::::::::::::::::::::::::::::::::::::::

## OnDemand Web Interface

The OnDemand web interface at [https://ondemand.hpc.pomona.edu](https://ondemand.hpc.pomona.edu) provides graphical file management without command-line tools.

**Best for**: Small files (under 500 MB), interactive browsing, one-off transfers.

**Limitations**: File size limit (~2 GB per upload), slower than rsync, not suitable for batch operations.

::::::::::::::::::::::::::::::::::::: callout

## When to Use Web vs. Command-Line

Use OnDemand for small files under 500 MB and interactive file browsing. Use rsync for files larger than 500 MB, batch operations, and automated transfers.

::::::::::::::::::::::::::::::::::::::::::::::::::

## Accessing OnDemand

1. Navigate to `https://ondemand.hpc.pomona.edu`
2. Login with your Pomona College credentials
3. Approve the Duo two-factor authentication push
4. Click **Files** in the navigation menu

The file manager shows your home directory by default with buttons for Home, New File, New Dir, and Upload.

## Uploading Files

1. Navigate to the destination folder (e.g., `/rhome/<myusername>`)
2. Click the **Upload** button
3. Select file(s) from your computer
4. Wait for the progress bar to complete

For multiple files, hold Ctrl (Windows) or Cmd (Mac) to select several at once. To upload a directory structure, create the folders on Sagehen first, then upload files into each.

For files larger than 500 MB, use rsync instead:
```bash
rsync -avhP ~/my_large_file.tar.gz <myusername>@sagehen.hpc.pomona.edu:/rhome/<myusername>/
```

## Downloading Files

1. Locate the file in the file manager
2. Right-click and select **Download**
3. The file saves to your Downloads folder

For directories, create a compressed archive first via SSH, then download the archive:
```bash
tar czf project_backup.tar.gz my_project/
```

## Organizing Files

- **Create directory**: Click "+ New Dir", enter name
- **Create file**: Click "+ New File", enter name
- **Delete**: Right-click, select Delete (usually permanent)
- **Rename**: Right-click, select Rename (if available)

::::::::::::::::::::::::::::::::::::: challenge

## Challenge: Upload and Download

1. Create a folder structure on Sagehen:
   - `/rhome/<myusername>/workshop/data/`
   - `/rhome/<myusername>/workshop/results/`
2. Upload any file from your computer to the `data/` folder
3. Refresh the browser (F5) and verify it appears
4. Download the file back to your computer and confirm it matches

::::::::::::::::::::::::::::::::::::: solution

After completing the challenge, you should see your uploaded file in the OnDemand file manager under `/rhome/<myusername>/workshop/data/` with the correct file size. The downloaded copy should be identical to the original.

If the file does not appear after upload, try refreshing the page. If upload fails, check that the file is under 2 GB and your quota is not exceeded.

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- OnDemand at https://ondemand.hpc.pomona.edu provides GUI-based file management
- Suitable for small files (under 500 MB) and interactive browsing
- Always refresh the browser after uploads to verify success
- Use rsync for large files or batch operations

::::::::::::::::::::::::::::::::::::::::::::::::::

<!-- highlight <labname>/<myusername> placeholders in code blocks; remove if the varnish theme handles this natively -->
<script>(function(){var CSS='.sh-placeholder{color:#c2410c;font-weight:700}[data-bs-theme="dark"] .sh-placeholder,html.dark .sh-placeholder{color:#fdba74}@media (prefers-color-scheme: dark){[data-bs-theme="auto"] .sh-placeholder{color:#fdba74}}';var RX=/<labname>|<myusername>/g;function firstMatch(el){var w=document.createTreeWalker(el,NodeFilter.SHOW_TEXT,null),nodes=[],full='';while(w.nextNode()){nodes.push({n:w.currentNode,s:full.length});full+=w.currentNode.nodeValue;}RX.lastIndex=0;var m;while((m=RX.exec(full))){var s=m.index,e=s+m[0].length,inSpan=false,parts=[];for(var j=0;j<nodes.length;j++){var ns=nodes[j].s,ne=ns+nodes[j].n.nodeValue.length;if(ne<=s||ns>=e)continue;parts.push({node:nodes[j].n,a:Math.max(s-ns,0),b:Math.min(e-ns,nodes[j].n.nodeValue.length)});var p=nodes[j].n.parentNode;while(p&&p!==el){if(p.classList&&p.classList.contains('sh-placeholder')){inSpan=true;break;}p=p.parentNode;}}if(!inSpan&&parts.length)return parts;}return null;}function wrapParts(parts){for(var i=parts.length-1;i>=0;i--){var t=parts[i].node,txt=t.nodeValue,a=parts[i].a,b=parts[i].b;var span=document.createElement('span');span.className='sh-placeholder';span.textContent=txt.slice(a,b);var f=document.createDocumentFragment();if(a>0)f.appendChild(document.createTextNode(txt.slice(0,a)));f.appendChild(span);if(b<txt.length)f.appendChild(document.createTextNode(txt.slice(b)));t.parentNode.replaceChild(f,t);}}function run(){var st=document.createElement('style');st.textContent=CSS;document.head.appendChild(st);document.querySelectorAll('pre,code').forEach(function(el){var guard=0,parts;while((parts=firstMatch(el))&&guard++<500){wrapParts(parts);}});}if(document.readyState==='loading'){document.addEventListener('DOMContentLoaded',run);}else{run();}})();</script>
