---
title: "Using tmpfs RAM Storage"
teaching: 20
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- When should I use `/tmpfs` instead of `/scratch`?
- How do I use RAM-backed storage safely in job scripts?
- What are the limitations of tmpfs?

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

After completing this episode, participants will be able to:
- Explain the difference between `/scratch` and `/tmpfs`
- Use `/tmpfs` for memory-intensive I/O workloads
- Account for memory overhead when sizing tmpfs usage
- Implement defensive patterns to prevent data loss

::::::::::::::::::::::::::::::::::::::::::::::::::

## What Is tmpfs?

`/tmpfs` is RAM-backed storage -- the fastest storage available on Sagehen. Data is stored directly in node memory, providing speeds over 1000 MB/s compared to 100-300 MB/s for scratch.

**Key constraints:**
- Limited to your job's memory allocation
- Single compute node only (not multi-node)
- **Deleted immediately when job terminates**

## When to Use tmpfs vs. Scratch

```
Does your working data fit in allocated memory?
    +-- YES, and single-node job
    |   +-- Use /tmpfs (fastest)
    +-- NO, or multi-node job
        +-- Use /scratch (very fast, larger capacity)
```

| Aspect | /scratch | /tmpfs |
|--------|----------|--------|
| **Speed** | Very fast (SSD) | Fastest (RAM) |
| **Size limit** | Disk available | Job memory allocation |
| **Persistence** | Deleted when job completes | Deleted when job completes |
| **Multi-node** | Yes | No (single node only) |
| **Best for** | Large working datasets | Small, rapid read-write data |

## Using tmpfs in Job Scripts

```bash
#!/bin/bash
#SBATCH --job-name=intensive
#SBATCH --time=01:00:00
#SBATCH --nodes=1
#SBATCH --mem=64G

TMPFS=/tmpfs/$SLURM_JOB_USER/$SLURM_JOB_ID
mkdir -p $TMPFS

# Copy small input (must fit in available memory)
cp /rhome/<myusername>/small_config.json $TMPFS/

# Build index in RAM-backed storage
./build_index /rhome/<myusername>/reference.fasta -o $TMPFS/index/

# Run analysis using fast in-memory data
./analyze -index $TMPFS/index/ -query /rhome/<myusername>/queries.fasta \
    -o /rhome/<myusername>/results/output.txt
```

::::::::::::::::::::::::::::::::::::: callout

## Planning Memory for tmpfs

When using `/tmpfs`, account for OS and application overhead:
- If you request `--mem=48G`, allocate only ~40GB to tmpfs
- Leave headroom for system processes and your application's memory usage
- Monitor with `free` during development runs

::::::::::::::::::::::::::::::::::::::::::::::::::

## Defensive Job Script Pattern

Always ensure results are copied back before the job ends:

```bash
#!/bin/bash
#SBATCH --job-name=safe_tmpfs
#SBATCH --time=02:00:00
#SBATCH --mem=32G

set -e  # Exit on any error

TMPFS=/tmpfs/$SLURM_JOB_USER/$SLURM_JOB_ID
HOME_DIR=/rhome/<myusername>
RESULTS=$HOME_DIR/results_$(date +%Y%m%d_%H%M%S)
mkdir -p $TMPFS $RESULTS

# Trap errors
trap 'echo "ERROR at line $LINENO"; exit 1' ERR

# Computation in tmpfs
cd $TMPFS
./analysis > output.dat

# CRITICAL: Copy before job ends
rsync -avhP $TMPFS/output.dat $RESULTS/ || { echo "Copy failed!"; exit 1; }

echo "Success: Results saved to $RESULTS"
```

## Common Mistakes

### Forgetting to copy results back

```bash
# WRONG: output.dat disappears when job ends
cd /tmpfs/$SLURM_JOB_USER/$SLURM_JOB_ID
./analysis > output.dat
# Job ends -- data is gone!

# CORRECT: copy before job ends
cd /tmpfs/$SLURM_JOB_USER/$SLURM_JOB_ID
./analysis > output.dat
cp output.dat /rhome/<myusername>/results/
```

### Exceeding memory allocation

If your data plus application memory exceeds the `--mem` allocation, the job will be killed by SLURM. Plan conservatively.

::::::::::::::::::::::::::::::::::::: challenge

## Challenge: Design a tmpfs Job Script

Write a job script for this scenario: You have a 5 GB reference index that needs rapid random access during a 1-hour analysis. The analysis itself uses about 8 GB of RAM.

Your script should:
1. Request appropriate memory (index + application + overhead)
2. Copy the index to tmpfs
3. Run the analysis
4. Copy results back to permanent storage

::::::::::::::::::::::::::::::::::::: solution

```bash
#!/bin/bash
#SBATCH --job-name=fast_analysis
#SBATCH --time=01:00:00
#SBATCH --nodes=1
#SBATCH --mem=20G  # 5GB index + 8GB app + 7GB overhead

set -e

TMPFS=/tmpfs/$SLURM_JOB_USER/$SLURM_JOB_ID
mkdir -p $TMPFS

# Copy index to fast RAM storage
rsync -avhP /rhome/<myusername>/reference_index/ $TMPFS/index/

# Run analysis with RAM-backed index
./analyze -index $TMPFS/index/ -o $TMPFS/results.txt

# Copy results back before job ends
rsync -avhP $TMPFS/results.txt /rhome/<myusername>/results/
```

The 20 GB memory request covers the 5 GB index, 8 GB application, and 7 GB of overhead for the OS and filesystem.

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- `/tmpfs` is RAM-backed storage providing the fastest I/O on Sagehen
- Limited by job memory allocation and restricted to single-node jobs
- Data on `/tmpfs` is deleted immediately when the job completes
- Always copy results to permanent storage before the job ends
- Account for OS overhead when planning tmpfs memory usage

::::::::::::::::::::::::::::::::::::::::::::::::::

<!-- highlight <labname>/<myusername> placeholders in code blocks; remove if the varnish theme handles this natively -->
<script>(function(){var CSS='.sh-placeholder{color:#c2410c;font-weight:700}[data-bs-theme="dark"] .sh-placeholder,html.dark .sh-placeholder{color:#fdba74}@media (prefers-color-scheme: dark){[data-bs-theme="auto"] .sh-placeholder{color:#fdba74}}';var RX=/<labname>|<myusername>/g;function firstMatch(el){var w=document.createTreeWalker(el,NodeFilter.SHOW_TEXT,null),nodes=[],full='';while(w.nextNode()){nodes.push({n:w.currentNode,s:full.length});full+=w.currentNode.nodeValue;}RX.lastIndex=0;var m;while((m=RX.exec(full))){var s=m.index,e=s+m[0].length,inSpan=false,parts=[];for(var j=0;j<nodes.length;j++){var ns=nodes[j].s,ne=ns+nodes[j].n.nodeValue.length;if(ne<=s||ns>=e)continue;parts.push({node:nodes[j].n,a:Math.max(s-ns,0),b:Math.min(e-ns,nodes[j].n.nodeValue.length)});var p=nodes[j].n.parentNode;while(p&&p!==el){if(p.classList&&p.classList.contains('sh-placeholder')){inSpan=true;break;}p=p.parentNode;}}if(!inSpan&&parts.length)return parts;}return null;}function wrapParts(parts){for(var i=parts.length-1;i>=0;i--){var t=parts[i].node,txt=t.nodeValue,a=parts[i].a,b=parts[i].b;var span=document.createElement('span');span.className='sh-placeholder';span.textContent=txt.slice(a,b);var f=document.createDocumentFragment();if(a>0)f.appendChild(document.createTextNode(txt.slice(0,a)));f.appendChild(span);if(b<txt.length)f.appendChild(document.createTextNode(txt.slice(b)));t.parentNode.replaceChild(f,t);}}function run(){var st=document.createElement('style');st.textContent=CSS;document.head.appendChild(st);document.querySelectorAll('pre,code').forEach(function(el){var guard=0,parts;while((parts=firstMatch(el))&&guard++<500){wrapParts(parts);}});}if(document.readyState==='loading'){document.addEventListener('DOMContentLoaded',run);}else{run();}})();</script>
