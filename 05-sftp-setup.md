---
title: "SFTP Setup with FileZilla"
teaching: 20
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- What is FileZilla and why use it for file transfers?
- How do I install and configure FileZilla?
- How do I connect to Sagehen HPC with Duo authentication?

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

After completing this episode, participants will be able to:
- Install FileZilla on their personal computer
- Connect to Sagehen's SFTP server with Duo authentication
- Save connection settings in Site Manager for future use

::::::::::::::::::::::::::::::::::::::::::::::::::

## Why FileZilla?

**FileZilla** is a free, open-source graphical SFTP client with side-by-side local and remote directory views, drag-and-drop transfers, and resume capability for interrupted transfers.

::::::::::::::::::::::::::::::::::::: callout

## FileZilla vs. rsync

Choose FileZilla for interactive transfers under 1 GB when you want a graphical interface and to browse both directories simultaneously. Choose rsync for large files over 1 GB, batch transfers, and automated workflows.

::::::::::::::::::::::::::::::::::::::::::::::::::

## Installing FileZilla

### Windows

1. Visit [filezilla-project.org](https://filezilla-project.org/download.php?type=client)
2. Download and run the installer
3. Follow the installation wizard

### macOS

1. Download the macOS version from [filezilla-project.org](https://filezilla-project.org/download.php?type=client)
2. Open the .dmg file
3. Drag FileZilla to Applications

### Linux

```bash
# Ubuntu/Debian
sudo apt-get install filezilla

# Fedora/RHEL
sudo dnf install filezilla
```

## Connecting to Sagehen HPC

### Quick Connect

In the Quick Connect bar at the top of FileZilla:

- **Host**: `sagehen.hpc.pomona.edu`
- **Username**: Your Pomona username
- **Password**: Your Pomona password
- **Port**: `22`

Click **Quickconnect**. When prompted for Duo 2FA, approve the push notification on your phone. The remote file panel then shows your home directory.

### Site Manager (Recommended)

For regular connections, save a site profile:

1. Menu > File > Site Manager (or Ctrl+S)
2. Click **New Site**, name it "Sagehen"
3. Set Host: `sagehen.hpc.pomona.edu`
4. Protocol: **SFTP - SSH File Transfer Protocol**
5. Logon Type: **Ask for password**
6. User: your username
7. Click **Connect**

This saves the connection so you only need to click Connect and enter your password each time.

### Duo Authentication Flow

1. Enter username and password in FileZilla
2. SSH connection initiates
3. Duo push notification arrives on your phone
4. Approve on your phone
5. Connection established

If no Duo prompt appears, check your phone for delayed notifications, try reconnecting, or contact its-hpc@pomona.edu.

::::::::::::::::::::::::::::::::::::: challenge

## Challenge: Connect to Sagehen HPC

1. Install FileZilla on your computer
2. Connect using Quick Connect:
   - Host: `sagehen.hpc.pomona.edu`, Port: 22
   - Enter your Pomona credentials
   - Approve Duo 2FA
3. Verify the remote panel shows your home directory files
4. Save the connection in Site Manager for future use

::::::::::::::::::::::::::::::::::::: solution

FileZilla should show "Connected to sagehen.hpc.pomona.edu" in the title bar and display your files from `/rhome/<myusername>` in the remote panel. If connection fails, verify the hostname is exactly `sagehen.hpc.pomona.edu`, port is 22, and check for Duo notification on your phone.

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- FileZilla is a free graphical SFTP client for interactive file transfers
- Connect to `sagehen.hpc.pomona.edu` on port 22 using SFTP protocol
- Duo 2FA is required for all connections
- Save connection settings in Site Manager for convenience

::::::::::::::::::::::::::::::::::::::::::::::::::

<!-- highlight <labname>/<myusername> placeholders in code blocks; remove if the varnish theme handles this natively -->
<script>(function(){var CSS='.sh-placeholder{color:#c2410c;font-weight:700}[data-bs-theme="dark"] .sh-placeholder,html.dark .sh-placeholder{color:#fdba74}@media (prefers-color-scheme: dark){[data-bs-theme="auto"] .sh-placeholder{color:#fdba74}}';var RX=/<labname>|<myusername>/g;function firstMatch(el){var w=document.createTreeWalker(el,NodeFilter.SHOW_TEXT,null),nodes=[],full='';while(w.nextNode()){nodes.push({n:w.currentNode,s:full.length});full+=w.currentNode.nodeValue;}RX.lastIndex=0;var m;while((m=RX.exec(full))){var s=m.index,e=s+m[0].length,inSpan=false,parts=[];for(var j=0;j<nodes.length;j++){var ns=nodes[j].s,ne=ns+nodes[j].n.nodeValue.length;if(ne<=s||ns>=e)continue;parts.push({node:nodes[j].n,a:Math.max(s-ns,0),b:Math.min(e-ns,nodes[j].n.nodeValue.length)});var p=nodes[j].n.parentNode;while(p&&p!==el){if(p.classList&&p.classList.contains('sh-placeholder')){inSpan=true;break;}p=p.parentNode;}}if(!inSpan&&parts.length)return parts;}return null;}function wrapParts(parts){for(var i=parts.length-1;i>=0;i--){var t=parts[i].node,txt=t.nodeValue,a=parts[i].a,b=parts[i].b;var span=document.createElement('span');span.className='sh-placeholder';span.textContent=txt.slice(a,b);var f=document.createDocumentFragment();if(a>0)f.appendChild(document.createTextNode(txt.slice(0,a)));f.appendChild(span);if(b<txt.length)f.appendChild(document.createTextNode(txt.slice(b)));t.parentNode.replaceChild(f,t);}}function run(){var st=document.createElement('style');st.textContent=CSS;document.head.appendChild(st);document.querySelectorAll('pre,code').forEach(function(el){var guard=0,parts;while((parts=firstMatch(el))&&guard++<500){wrapParts(parts);}});}if(document.readyState==='loading'){document.addEventListener('DOMContentLoaded',run);}else{run();}})();</script>
