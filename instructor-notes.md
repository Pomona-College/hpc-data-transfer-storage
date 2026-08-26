# Instructor Guide

## Workshop Overview

This 4-5 hour workshop teaches researchers how to efficiently manage data on the Sagehen HPC cluster. The progression moves from foundational concepts (storage locations, quotas) to practical tools (transfer methods) to advanced topics (temporary storage optimization, best practices).

## Target Audience

- Pomona College researchers using Sagehen HPC
- Diverse backgrounds: biology, chemistry, physics, CS, engineering
- Varying HPC experience: novice to experienced users
- Mix of CLI and GUI preference

## Learning Outcomes

After this workshop, participants will:

1. Understand Sagehen's storage hierarchy and when to use each location
2. Monitor storage quotas and manage disk space
3. Transfer files using multiple methods (OnDemand, FileZilla, rsync)
4. Use temporary storage in job scripts for performance
5. Implement data organization and backup strategies

## Time Allocation

**Total: 4-5 hours including breaks**

- Episode 1 (Storage Hierarchy): 45 min teaching + 15 min exercises
- Episode 2 (Quotas): 45 min teaching + 15 min exercises
- Episode 3 (OnDemand): 40 min teaching + 20 min exercises
- *BREAK: 10 minutes*
- Episode 4 (FileZilla): 50 min teaching + 20 min exercises
- Episode 5 (rsync): 60 min teaching + 25 min exercises
- *BREAK: 10 minutes*
- Episode 6 (Temporary Storage): 50 min teaching + 20 min exercises
- Episode 7 (Best Practices): 45 min teaching + 15 min exercises

**Flexible timing:** Adjust based on audience needs and questions

## Pre-Workshop Setup

### Instructor Preparation (1-2 hours before)

1. **Test Connections:**
   - SSH to Sagehen: Verify connectivity
   - Test OnDemand: https://ondemand.sagehen.hpc.pomona.edu
   - Open FileZilla: Test connection to Sagehen
   - Verify rsync: Test transfer with sample file

2. **Prepare Demo Files:**
   ```bash
   # Create test files in /rhome/<myusername>/demo/
   mkdir -p /rhome/<myusername>/demo/{input,output}

   # Sample input files
   dd if=/dev/zero of=/rhome/<myusername>/demo/input/sample_100mb.bin bs=1M count=100
   echo "Test data" > /rhome/<myusername>/demo/input/small.txt

   # Sample job script
   cat > /rhome/<myusername>/demo/test_job.sh << 'EOF'
   #!/bin/bash
   #SBATCH --time=00:10:00
   echo "Sample job output at $(date)"
   EOF
   ```

3. **Load Workshop Materials:**
   - Have all workshop markdown files accessible
   - Test any embedded links/URLs
   - Print reference sheet (optional for handout)

4. **Prepare Screen Share Setup:**
   - Arrange monitors or test screen sharing
   - Test audio and microphone
   - Have backup plan if projector fails

5. **Check Roster:**
   - Know approximate number of attendees
   - Note any accessibility requirements
   - Prepare introductions if expected

### Day-Of Checklist (30 minutes before)

- [ ] Test SSH connection works
- [ ] Verify OnDemand web interface accessible
- [ ] FileZilla connection test (if including Episode 4)
- [ ] rsync test transfer (if including Episode 5)
- [ ] Projector/screen sharing working
- [ ] Audio/microphone test
- [ ] Demo files accessible
- [ ] Reference materials printed or accessible
- [ ] Contact numbers posted (in case of technical issues)

## Teaching Approach

### Pedagogical Strategy

This workshop uses **active learning** with multiple modalities:

1. **Direct Instruction:** Explain concepts (45-50 minutes per episode)
2. **Demonstration:** Show how tools work in real-time (integrated)
3. **Guided Practice:** Exercises with instructor support (15-25 minutes)
4. **Peer Learning:** Participants help each other troubleshoot

### Presentation Tips

**For Storage Hierarchy (Episode 1):**
- Draw the storage hierarchy on whiteboard/screen while explaining
- Use real `/rhome/<myusername>` and `/bigdata/lab/<labname>` paths
- Show actual filesystem layout on Sagehen
- Have participants locate their own directories during demo

**For Quotas (Episode 2):**
- Run `quota_check.sh` live, show output interpretation
- Explain why `--apparent-size` flag matters on BeeGFS
- Walk through finding large files on actual home directory
- Show real quota warning email as example

**For OnDemand (Episode 3):**
- Screen share and upload a file while participants follow
- Show file appearing in directory after refresh
- Emphasize F5 refresh requirement (common issue)
- Have participants upload own file during exercise

**For FileZilla (Episode 4):**
- Install during lesson if not done beforehand
- Show connection success message
- Demonstrate drag-and-drop with real file
- Show transfer queue with speeds
- Have participants test connection

**For rsync (Episode 5):**
- Show a complete rsync cycle (upload + verify)
- Demonstrate --dry-run before actual transfer
- Show progress bar output
- Pause transfer mid-way to show resume capability
- Explain trailing slash with examples on board

**For Temporary Storage (Episode 6):**
- Show SLURM variables in real job script
- Submit demo job and show storage in action
- Monitor job with quota_check.sh during execution
- Show data deletion after job ends (important lesson!)

**For Best Practices (Episode 7):**
- Use examples from participant's actual research areas
- Show real directory organization structure
- Discuss quota issues your institution faces
- Share troubleshooting war stories

## Common Issues and Solutions

### Issue: Participants Can't Access Sagehen HPC

**Problem:** SSH connection fails, OnDemand login error

**Prevention:**
- Collect usernames in advance, verify accounts exist
- Test Sagehen uptime before workshop
- Have IT contact info ready

**During Workshop:**
- Have those participants use instructor's terminal via screen share
- Provide printouts of key commands
- Offer follow-up support email

### Issue: Slow Network During File Transfers

**Problem:** rsync/FileZilla transfer takes much longer than expected

**Prevention:**
- Use smaller test files (10 MB instead of 100 MB)
- Demo on pre-uploaded files instead of live transfer
- Have backup video/animation of transfers

**During Workshop:**
- Show speed calculation formula: Size / Time = Speed
- Discuss why cluster network may be congested
- Skip large transfer demo if taking >5 minutes

### Issue: Duo Authentication Delays

**Problem:** No Duo prompt, or 30+ second delay

**Prevention:**
- Allow extra time for Duo in schedule
- Have phone ready with Duo app open
- Test Duo enrollment before workshop

**During Workshop:**
- Have backup: show expected Duo screen on screenshot
- Explain Duo may take 5-10 seconds
- Participant with phone issues can observe others

### Issue: FileZilla Installation Fails on Windows

**Problem:** Administrator permissions required or antivirus blocks

**Prevention:**
- Note in setup.md that FileZilla is optional
- Have alternative (rsync command-line) ready
- Suggest WSL installation if time allows

**During Workshop:**
- Skip FileZilla install, demonstrate only
- Focus on rsync which works on Windows via WSL
- Offer post-workshop installation support

### Issue: Participants Confused About Trailing Slashes

**Problem:** Many people don't understand why `dir/` is different from `dir`

**Prevention:**
- Spend extra time on this in rsync episode
- Use diagrams/whiteboard
- Provide visual examples

**During Workshop:**
- Draw source and destination on board
- Show with actual example
- Create 3-4 practice examples they try
- This is important enough to take time!

### Issue: Participants Lose Data in Tmpfs Demo

**Problem:** Someone doesn't copy results from /tmpfs before job ends

**Prevention:**
- Emphasize multiple times
- Use different colors/bold in slides
- Show the error message of missing files

**During Workshop:**
- Make it a learning moment, not a failure
- Explain this is THE most important lesson
- Have them re-run with copying results
- Reinforce: always copy from tmpfs!

## Content Customization

Depending on audience, you can adjust emphasis:

### For Beginners (Mostly New Users)
- Spend more time on Episode 1 (storage basics)
- Emphasize OnDemand (Episode 3) over command-line
- Keep rsync (Episode 5) simpler
- Strong focus on avoiding common pitfalls

### For Experienced Users (Many PhD Researchers)
- Spend less time on Episode 1
- Quickly cover OnDemand, jump to FileZilla/rsync
- Deep dive into rsync patterns and optimization
- Advanced Episode 6 (temporary storage in complex jobs)
- Episode 7 best practices for research groups

### For Instructors Teaching Courses
- Customize Example 5 in Episode 7 (class data organization)
- Discuss managing quotas with 20+ students
- Sharing class datasets and permissions
- Collecting and archiving student work

### For Research Groups
- Emphasize collaborative storage (/bigdata)
- Lab data organization and sharing
- Automation scripts for group workflows
- Backup strategies for group data

## Accessibility Considerations

**For Hearing Impaired:**
- Provide captions/transcript of key concepts
- Use visual diagrams extensively
- Written handouts of all commands

**For Visually Impaired:**
- Describe screen elements verbally
- Provide text versions of diagrams
- Use large fonts if projecting
- Accessibility mode in browsers

**For Different Learning Styles:**
- Visual: Diagrams, flowcharts, screen shares
- Auditory: Explanations, Q&A, discussion
- Kinesthetic: Hands-on exercises, interactive demos
- Reading/Writing: Handouts, reference guides, markdown files

## Post-Workshop Support

### Follow-Up Materials

Provide participants:
- Download link to workshop materials
- Reference sheet (PDF)
- Quick reference guide
- Setup instructions for home use

### Support Channel

Create avenue for follow-up questions:
- Email to its-hpc@pomona.edu
- Optional: office hours for HPC questions
- Slack channel for HPC community (if exists)
- Documentation wiki for common issues

### Assessment

Consider surveying:
- Which topics most useful
- Which topics need more time
- Quality of instruction
- Pacing and timing
- What follow-up training needed

**Sample Survey Question:**
```
1. I can now organize my data files on Sagehen
   Strongly Disagree / Disagree / Neutral / Agree / Strongly Agree

2. I understand when to use /scratch vs /tmpfs
   [ ] Yes [ ] Somewhat [ ] No

3. I can transfer files between my computer and Sagehen
   [ ] Yes [ ] Somewhat [ ] No

4. Which topics need more explanation? (open text)
```

## Resources for Instructors

### If You're New to Sagehen HPC

- Request account and hands-on access week before
- Meet with HPC staff (Andrew Wilson) to learn infrastructure
- Review quota_check.sh behavior on actual cluster
- Test all command examples on real Sagehen

### Training Materials

- This instructor guide
- Episode markdown files (7 total)
- Reference sheet
- Learner profiles
- Setup instructions

### Getting Help

- Contact: its-hpc@pomona.edu
- HPC Coordinator: Andrew Wilson
- IT Help: servicedesk@pomona.edu

### Related Training

Participants may benefit from:
- Basic Linux command-line training (prerequisite)
- Git version control (complements backup strategies)
- SLURM job submission (complements temporary storage)
- Python scripting (for rsync automation)

## Troubleshooting During Workshop

### If You Forget a Command

- Have reference sheet visible
- Open episode markdown in browser
- Ask participants (they may know!)
- It's fine to look things up

### If Stuck on a Question

- Repeat the question back
- Admit if you don't know
- Offer to research and email answer
- Use as opportunity for discussion

### If Technology Fails

- Have backup materials printed
- Can teach without projector (use whiteboard)
- Can do demo on shared terminal instead of screenshare
- Participants can follow along via printed handouts

### If Running Behind

- Reduce time on earlier episodes if early ones running long
- Skip optional demos (rsync --delete can be mentioned, not demoed)
- Move advanced content to office hours
- Extend workshop to next day if multi-day event possible

## Follow-Up Ideas

### For Engaged Participants

- Invite to office hours for advanced topics
- Suggest advanced workshop: "Automation and Scripting"
- Recommend they attend cluster update/upgrade training
- Ask if interested in mentoring other users

### For Those Struggling

- Offer 1-on-1 follow-up consultation
- Share personalized reference materials
- Recommend prerequisite training
- Check in via email after workshop

### For Building Community

- Create mailing list for HPC users
- Regular office hours (posted schedule)
- Slack/Teams channel for collaboration
- Advanced workshops annually

## Instructor Self-Care

Teaching technical material is demanding:

- Start workshop fresh (good night's sleep)
- Have water and snacks available
- Take breaks (don't skip them)
- It's okay to not know everything
- Mistakes are teaching opportunities
- Acknowledge good questions with enthusiasm

Remember: You're providing valuable skills that will enhance their research. Thank you for teaching!

---

**Questions or suggestions?** Contact its-hpc@pomona.edu

<!-- highlight <labname>/<myusername> placeholders in code blocks; remove if the varnish theme handles this natively -->
<script>(function(){var CSS='.sh-placeholder{color:#c2410c;font-weight:700}[data-bs-theme="dark"] .sh-placeholder,html.dark .sh-placeholder{color:#fdba74}@media (prefers-color-scheme: dark){[data-bs-theme="auto"] .sh-placeholder{color:#fdba74}}';var RX=/<labname>|<myusername>/g;function firstMatch(el){var w=document.createTreeWalker(el,NodeFilter.SHOW_TEXT,null),nodes=[],full='';while(w.nextNode()){nodes.push({n:w.currentNode,s:full.length});full+=w.currentNode.nodeValue;}RX.lastIndex=0;var m;while((m=RX.exec(full))){var s=m.index,e=s+m[0].length,inSpan=false,parts=[];for(var j=0;j<nodes.length;j++){var ns=nodes[j].s,ne=ns+nodes[j].n.nodeValue.length;if(ne<=s||ns>=e)continue;parts.push({node:nodes[j].n,a:Math.max(s-ns,0),b:Math.min(e-ns,nodes[j].n.nodeValue.length)});var p=nodes[j].n.parentNode;while(p&&p!==el){if(p.classList&&p.classList.contains('sh-placeholder')){inSpan=true;break;}p=p.parentNode;}}if(!inSpan&&parts.length)return parts;}return null;}function wrapParts(parts){for(var i=parts.length-1;i>=0;i--){var t=parts[i].node,txt=t.nodeValue,a=parts[i].a,b=parts[i].b;var span=document.createElement('span');span.className='sh-placeholder';span.textContent=txt.slice(a,b);var f=document.createDocumentFragment();if(a>0)f.appendChild(document.createTextNode(txt.slice(0,a)));f.appendChild(span);if(b<txt.length)f.appendChild(document.createTextNode(txt.slice(b)));t.parentNode.replaceChild(f,t);}}function run(){var st=document.createElement('style');st.textContent=CSS;document.head.appendChild(st);document.querySelectorAll('pre,code').forEach(function(el){var guard=0,parts;while((parts=firstMatch(el))&&guard++<500){wrapParts(parts);}});}if(document.readyState==='loading'){document.addEventListener('DOMContentLoaded',run);}else{run();}})();</script>
