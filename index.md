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
| `/rhome/username` | Home directory | Lab quota | Permanent |
| `/bigdata/labname` | Shared lab storage | 1TB per lab | Permanent |
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

This workshop consists of 7 episodes covering progressively more advanced topics:

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
