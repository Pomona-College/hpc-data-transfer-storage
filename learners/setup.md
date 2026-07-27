---
title: Setup
---

## Pre-Workshop Requirements

This workshop teaches you how to efficiently transfer data to and from the Sagehen HPC cluster and manage your storage allocations. Before the workshop, please ensure you have completed the following setup steps.

## System Requirements

You should have a computer running Windows, macOS, or Linux with a stable internet connection. Administrative privileges may be required for software installation.

## Software Installation

### 1. SSH Client

You'll need an SSH client to connect to the Sagehen cluster. Choose the option below for your operating system:

:::::::::::::::: solution

### Windows

**Option A: PuTTY (Recommended for beginners)**
1. Download PuTTY from https://www.putty.org/
2. Run the installer and follow the prompts
3. You can also install Pageant (comes with PuTTY) for SSH key management

**Option B: OpenSSH (Windows 10+)**
1. Open PowerShell as Administrator
2. Run: `Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*' | Add-WindowsCapability -Online`
3. Verify installation: Open Command Prompt and type `ssh`

**Option C: Windows Terminal + Git Bash**
1. Install Windows Terminal from the Microsoft Store
2. Install Git for Windows from https://git-scm.com/download/win
3. Open Git Bash and test with `ssh`

:::::::::::::::::::::::::

:::::::::::::::: solution

### macOS

OpenSSH is built-in to macOS. Open Terminal and test your SSH installation:

```bash
ssh -V
```

You should see output like `OpenSSH_8.6p1, LibreSSL 3.3.6`. If not, install it via Homebrew:

```bash
brew install openssh
```

:::::::::::::::::::::::::

:::::::::::::::: solution

### Linux

SSH is typically pre-installed. Verify by opening a terminal and typing:

```bash
ssh -V
```

If not installed, use your package manager:

**Debian/Ubuntu:**
```bash
sudo apt-get install openssh-client
```

**RHEL/CentOS:**
```bash
sudo yum install openssh-clients
```

:::::::::::::::::::::::::

### 2. FileZilla (SFTP/FTP File Manager)

FileZilla provides a graphical interface for transferring files. Download it from https://filezilla-project.org/ and follow the installation wizard for your OS.

### 3. Rclone (Cloud & Remote Sync)

Rclone is a powerful command-line tool for syncing files with remote storage and Sagehen. Install it following the guide at https://rclone.org/install/

For **Windows**, **macOS**, and **Linux**, the simplest installation is:

```bash
curl https://rclone.org/install.sh | sudo bash
```

On **Windows**, you can also use Chocolatey:

```bash
choco install rclone
```

## SSH Key Setup

A public-private SSH key pair allows password-free authentication and is required for unattended transfers.

### Generate SSH Keys

If you don't already have SSH keys, generate a new pair:

```bash
ssh-keygen -t ed25519 -C "your.email@pomona.edu"
```

**Prompts:**
- **File location:** Press Enter to accept the default (`~/.ssh/id_ed25519`)
- **Passphrase:** Enter a strong passphrase or press Enter to skip (not recommended for security)

**Verify your keys:**

```bash
ls ~/.ssh/
```

You should see `id_ed25519` (private key) and `id_ed25519.pub` (public key).

### Add Your Public Key to Sagehen

Before the workshop, you should add your SSH public key to Sagehen. If your key is already configured (you've used Sagehen before), skip this step.

**Step 1:** Display your public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

Copy the entire output (should start with `ssh-ed25519`).

**Step 2:** Log into Sagehen with your password:

```bash
ssh your.username@sagehen.hpc.pomona.edu
```

Use your Pomona AD credentials. You'll be prompted for DUO MFA: approve it on your phone or use the Duo app.

**Step 3:** Create the `.ssh` directory if it doesn't exist:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

**Step 4:** Add your public key:

```bash
echo "your-public-key-here" >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

Paste the public key you copied in Step 1.

**Step 5:** Log out and test:

```bash
exit
```

Try logging in again without a password:

```bash
ssh your.username@sagehen.hpc.pomona.edu
```

If successful, you'll skip the password prompt (but may still be prompted for DUO MFA).

## Sagehen Access Test

Before the workshop, verify that you can access the Sagehen cluster:

1. **Connect via SSH:**
   ```bash
   ssh your.username@sagehen.hpc.pomona.edu
   ```

2. **Approve DUO MFA** on your registered device.

3. **Check your home directory:**
   ```bash
   pwd
   quota -s
   ```

4. **Verify you can see the storage locations:**
   ```bash
   ls /rhome/your.username
   ls /bigdata/
   ```

5. **Test OnDemand access:** Open https://ondemand.hpc.pomona.edu/ in your browser and log in with your Pomona AD credentials.

## Prepare Example Data

Having a few small test files will help during the workshop exercises:

1. Create a local test directory:
   ```bash
   mkdir -p ~/sagehen-test-data
   ```

2. Create some small test files:
   ```bash
   echo "Test file 1" > ~/sagehen-test-data/file1.txt
   echo "Test file 2" > ~/sagehen-test-data/file2.txt
   dd if=/dev/zero of=~/sagehen-test-data/sample-10mb.bin bs=1M count=10
   ```

3. Keep this directory handy for the workshop exercises.

## Helpful Resources

- **Sagehen Documentation:** https://sagehen.hpc.pomona.edu/docs/ (once you've accessed Sagehen)
- **HPC Support:** its-hpc@pomona.edu
- **OnDemand Portal:** https://ondemand.hpc.pomona.edu/
- **SSH Key Security:** https://wiki.archlinux.org/title/SSH_keys

## Troubleshooting

**Cannot connect to Sagehen?**
- Verify your Pomona AD username and password
- Check that you're on campus or connected to Pomona VPN
- Confirm DUO MFA is set up on your account

**DUO approval times out?**
- Ensure Duo app is updated on your phone
- Check your device's system time is synchronized
- Contact its-hpc@pomona.edu for account issues

**SSH keys not working?**
- Verify permissions: `ls -la ~/.ssh/` should show 700 for the directory and 600 for files
- Check that your public key was added correctly: `ssh -vvv your.username@sagehen.hpc.pomona.edu`
- Contact HPC support if issues persist

**FileZilla connection issues?**
- Host: `sagehen.hpc.pomona.edu`, Protocol: SFTP, Port: 22
- Use your Pomona AD username
- Try using your SSH key instead of password authentication

If you encounter issues during setup, please reach out to its-hpc@pomona.edu before the workshop.
