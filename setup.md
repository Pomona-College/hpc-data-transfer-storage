# Setup Instructions for Data Transfer and Storage Management Workshop

## Before the Workshop

To get the most out of this workshop, please complete these setup steps before attending.

## Prerequisites

This workshop assumes you have:

- An active Sagehen HPC cluster account
- Basic familiarity with Linux/Unix command-line interface
- A personal computer (Windows, macOS, or Linux)
- Internet connection

If you don't have a Sagehen account, request one at least one week before the workshop via the [HPC account request form](https://servicedesk.pomona.edu/support/catalog/items/83) (or by emailing its-hpc@pomona.edu)

## Software Installation

### 1. SSH Client

**macOS/Linux:**
SSH is pre-installed. Test by opening Terminal and running:
```bash
ssh -V
# Should show OpenSSH version
```

**Windows:**
- Option A: Use Windows Subsystem for Linux (WSL 2)
  - Install from Microsoft Store
  - Or follow: https://docs.microsoft.com/en-us/windows/wsl/install

- Option B: Use Windows 10/11 built-in OpenSSH
  - Settings → Apps → Optional Features
  - Search "OpenSSH" and install
  - Test in PowerShell: `ssh -V`

- Option C: Install PuTTY
  - Download from https://www.putty.org/
  - Graphical SSH client for Windows

### 2. FileZilla (Optional but Recommended)

Used in Episode 4 for graphical file transfers.

**Download:** https://filezilla-project.org/download.php?type=client

**Installation:**
- Windows: Run .exe installer
- macOS: Open .dmg and drag to Applications
- Linux: Use package manager (apt, dnf, etc.)

### 3. rsync

Used in Episode 5 for bulk file transfers.

**macOS:**
Pre-installed. Test with:
```bash
rsync --version
```

**Linux:**
Pre-installed on most distributions. Test with:
```bash
rsync --version
```

**Windows:**
- Install via WSL (recommended): `sudo apt-get install rsync`
- Or use Cygwin with rsync package

### 4. Web Browser

For OnDemand web interface in Episode 3.
- Chrome, Firefox, Safari, or Edge (all supported)
- Ensure JavaScript is enabled

## Account and Access Verification

### 1. Verify Sagehen Account Access

Test SSH connection to Sagehen:

```bash
ssh <myusername>@sagehen.hpc.pomona.edu
# Enter password when prompted
# Complete Duo 2FA authentication
# If successful, you'll see Sagehen login prompt
```

Type `exit` to log out.

### 2. Verify OnDemand Access

Test OnDemand web interface:

1. Open browser
2. Navigate to: https://ondemand.sagehen.hpc.pomona.edu
3. Login with Pomona credentials
4. Complete Duo authentication
5. You should see OnDemand dashboard

### 3. Verify Quota

Check your storage allocation:

```bash
ssh <myusername>@sagehen.hpc.pomona.edu
quota_check.sh
# Should show your current usage and quota
exit
```

## Create Test Data

### Option 1: Create Sample Files (Recommended)

Create some test files for the workshops:

```bash
# Create test directory
mkdir ~/test_data
cd ~/test_data

# Create small test file (for FileZilla and OnDemand examples)
echo "This is a test file" > small_file.txt

# Create medium test file (for rsync examples)
dd if=/dev/zero of=medium_file.bin bs=1M count=10
# Creates 10 MB file

# Create sample CSV data
cat > sample_data.csv << 'EOF'
id,name,value
1,sample_a,100
2,sample_b,200
3,sample_c,300
EOF
```

### Option 2: Use Existing Files

If you prefer, use any existing files on your computer:
- Documents (PDF, text)
- Data files (CSV, JSON)
- Images (JPG, PNG)

## Test File Transfer

Before the workshop, test one file transfer method:

### Test rsync (Recommended)

```bash
cd ~/test_data

# Upload test file
rsync -avhP small_file.txt <myusername>@sagehen.hpc.pomona.edu:/rhome/<myusername>/

# You should see:
# Connecting via SSH...
# Duo prompt (approve on phone)
# Transfer progress
# File listed on remote

# Verify on Sagehen
ssh <myusername>@sagehen.hpc.pomona.edu ls -lh /rhome/<myusername>/small_file.txt
```

### Test FileZilla (If Installed)

1. Open FileZilla
2. Enter in Quick Connect:
   - Host: sagehen.hpc.pomona.edu
   - Username: your_username
   - Port: 22
3. Click Quickconnect
4. Approve Duo
5. Should see home directory in right panel

### Test OnDemand

1. Navigate to: https://ondemand.sagehen.hpc.pomona.edu
2. Click "Files"
3. Should see your home directory

## Recommended Directory Structure

Before the workshop, create a project directory for workshop exercises:

```bash
mkdir -p ~/workshop_data/incoming ~/workshop_data/results
# incoming: For files we'll transfer INTO Sagehen
# results: Where we'll download results FROM Sagehen
```

## Troubleshooting Setup

### Cannot Connect via SSH

**Problem:** "Connection refused" or "Network unreachable"

**Solutions:**
1. Verify Sagehen is not down (check email)
2. Check hostname: `sagehen.hpc.pomona.edu`
3. Check internet connection: `ping google.com`
4. If behind corporate firewall, port 22 may be blocked
5. Contact its-hpc@pomona.edu for help

### Duo Authentication Not Working

**Problem:** No Duo prompt appears or authentication times out

**Solutions:**
1. Check phone is nearby and powered on
2. Check Duo app is installed and updated
3. Try again (may be delayed)
4. Check your Duo enrollment: https://accounts.pomona.edu/
5. Contact IT: its-help@pomona.edu

### FileZilla Won't Install

**Problem:** Permission error or installation fails

**Solutions:**
1. Run installer as administrator (Windows)
2. For macOS, drag to Applications folder in a new Finder window
3. Use package manager instead (Linux)
4. FileZilla is optional; OnDemand and command-line work without it

### rsync Not Found

**Problem:** "command not found" when typing `rsync`

**Solutions:**
1. **macOS:** Pre-installed, try full path: `/usr/bin/rsync`
2. **Linux:** Install with `sudo apt-get install rsync` or equivalent
3. **Windows:** Install WSL first, then rsync via WSL
4. rsync is optional; FileZilla works without it

## Bandwidth and Network Notes

This workshop involves file transfers. If on a slow connection:

- **Slow connection?** Download large test file overnight or before workshop
- **Limited bandwidth?** Skip large file transfer examples (instructor can demo)
- **On cellular?** Use WiFi for large transfers

## What to Bring

- Laptop with SSH client (Windows, macOS, or Linux)
- Pomona account credentials
- Phone for Duo authentication
- Notes/notebook for recording commands
- Optional: FileZilla if you want to follow graphical examples

## Getting Help

### Before Workshop

If you encounter setup issues:

1. **Check email:** Setup instructions may have updates
2. **Contact HPC Support:** its-hpc@pomona.edu
3. **Contact IT:** its-help@pomona.edu (for account/Duo issues)

### During Workshop

Instructors will help with any setup issues at the start of the workshop. Allow 15 minutes for everyone to verify access.

## Optional Pre-Workshop Review

To prepare for the workshop, consider reviewing:

1. **Basic Linux commands:** https://ubuntu.com/tutorials/command-line-for-beginners
2. **SSH explained:** https://www.ssh.com/ssh/command
3. **File transfer concepts:** https://en.wikipedia.org/wiki/Secure_file_transfer_protocol

## Ready?

If you've completed these steps, you're ready for the workshop! See you there.

Questions? Email its-hpc@pomona.edu

<!-- highlight <labname>/<myusername> placeholders in code blocks; remove if the varnish theme handles this natively -->
<script>(function(){var CSS='.sh-placeholder{color:#c2410c;font-weight:700}[data-bs-theme="dark"] .sh-placeholder,html.dark .sh-placeholder{color:#fdba74}@media (prefers-color-scheme: dark){[data-bs-theme="auto"] .sh-placeholder{color:#fdba74}}';var RX=/<labname>|<myusername>/g;function firstMatch(el){var w=document.createTreeWalker(el,NodeFilter.SHOW_TEXT,null),nodes=[],full='';while(w.nextNode()){nodes.push({n:w.currentNode,s:full.length});full+=w.currentNode.nodeValue;}RX.lastIndex=0;var m;while((m=RX.exec(full))){var s=m.index,e=s+m[0].length,inSpan=false,parts=[];for(var j=0;j<nodes.length;j++){var ns=nodes[j].s,ne=ns+nodes[j].n.nodeValue.length;if(ne<=s||ns>=e)continue;parts.push({node:nodes[j].n,a:Math.max(s-ns,0),b:Math.min(e-ns,nodes[j].n.nodeValue.length)});var p=nodes[j].n.parentNode;while(p&&p!==el){if(p.classList&&p.classList.contains('sh-placeholder')){inSpan=true;break;}p=p.parentNode;}}if(!inSpan&&parts.length)return parts;}return null;}function wrapParts(parts){for(var i=parts.length-1;i>=0;i--){var t=parts[i].node,txt=t.nodeValue,a=parts[i].a,b=parts[i].b;var span=document.createElement('span');span.className='sh-placeholder';span.textContent=txt.slice(a,b);var f=document.createDocumentFragment();if(a>0)f.appendChild(document.createTextNode(txt.slice(0,a)));f.appendChild(span);if(b<txt.length)f.appendChild(document.createTextNode(txt.slice(b)));t.parentNode.replaceChild(f,t);}}function run(){var st=document.createElement('style');st.textContent=CSS;document.head.appendChild(st);document.querySelectorAll('pre,code').forEach(function(el){var guard=0,parts;while((parts=firstMatch(el))&&guard++<500){wrapParts(parts);}});}if(document.readyState==='loading'){document.addEventListener('DOMContentLoaded',run);}else{run();}})();</script>
