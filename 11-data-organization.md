---
title: "Data Organization Best Practices"
teaching: 20
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- How should I organize my data on Sagehen HPC?
- What naming conventions should I follow?
- How do I share data effectively with collaborators?

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

After completing this episode, participants will be able to:
- Design efficient directory structures for research projects
- Apply meaningful naming conventions
- Set correct file permissions for collaboration
- Document data with README files

::::::::::::::::::::::::::::::::::::::::::::::::::

## Directory Structure

Organize data hierarchically by project and purpose:

```
/rhome/<myusername>/
+-- projects/
|   +-- project_alpha/
|   |   +-- data/
|   |   |   +-- raw/          (original, unmodified data)
|   |   |   +-- processed/    (cleaned, transformed data)
|   |   +-- code/             (scripts and programs)
|   |   +-- results/          (output, figures, statistics)
|   |   +-- README.md
|   +-- project_beta/
|       +-- data/
|       +-- code/
|       +-- results/
+-- archive/                  (compressed completed projects)

/bigdata/lab/<labname>/
+-- group_projects/            (collaborative work)
+-- lab_resources/             (shared reference data)
```

## Organizational Principles

**Separate data types**: Keep raw data separate from processed data and results. This enables clean, reproducible pipelines.

**Use meaningful names**: `dataset_v1.csv` is better than `data.csv`. Include version numbers or dates: `results_2024_03_05.txt`.

**Include metadata**: Add a `README.md` in each project explaining the structure, data sources, and how to reproduce the analysis.

**Archive completed projects**: Compress inactive projects with `tar czf project_2023.tar.gz project_2023/` and move to `/bigdata/lab/<labname>/archive/`.

## File Permissions for Collaboration

When sharing files through `/bigdata/lab/<labname>`, set appropriate permissions:

```bash
# Make files readable by lab members
chmod 644 /bigdata/lab/<labname>/shared_file.txt

# Make directories traversable
chmod 755 /bigdata/lab/<labname>/shared_dir/

# Set default permissions for future files
umask 0022  # Add to ~/.bashrc
```

## Sharing Data

### With Lab Members (Internal)

```bash
mkdir -p /bigdata/lab/<labname>/shared_results
cp analysis_output.csv /bigdata/lab/<labname>/shared_results/
chmod 644 /bigdata/lab/<labname>/shared_results/analysis_output.csv
```

Notify collaborators: "Results available in `/bigdata/lab/<labname>/shared_results/`"

### With External Collaborators

Compress and upload to a cloud service or institutional repository:

```bash
tar czf results_for_collaborators.tar.gz results/
```

### Publishing to a Repository

For public data release, prepare a clean package with data, metadata, a README, and a license file. Upload to your institutional repository or services like Zenodo.

## Avoiding Duplicate Data

Duplicate files waste quota. Use symbolic links instead of copies for shared reference data:

```bash
ln -s /bigdata/lab/<labname>/reference_data ~/reference
```

Find potential duplicates:

```bash
find /rhome/<myusername> -name "*.bak" -o -name "*_old" -o -name "*~"
```

::::::::::::::::::::::::::::::::::::: challenge

## Challenge: Design Your Data Organization

For a research project you are planning or working on:

1. Sketch a directory structure with folders for raw data, processed data, code, and results
2. Write a 3-line README.md explaining the project purpose and where to find key files
3. Identify one file or directory that could be moved to `/bigdata` for lab sharing

::::::::::::::::::::::::::::::::::::: solution

Example structure:
```
project_ml_classifier/
+-- data/raw/training_set_v1.csv
+-- data/processed/training_clean_v1.csv
+-- code/preprocessing.py
+-- code/training.py
+-- results/metrics.json
+-- results/figures/
+-- README.md
```

Good practices demonstrated: separation of raw and processed data, meaningful filenames with versions, code separate from data, README for documentation.

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Organize projects with separate directories for raw data, processed data, code, and results
- Use meaningful filenames with version numbers or dates
- Document projects with README files explaining structure and purpose
- Use `/bigdata/lab/<labname>` for collaborative data with appropriate permissions
- Use symbolic links instead of copies for shared reference data

::::::::::::::::::::::::::::::::::::::::::::::::::

<!-- highlight <labname>/<myusername> placeholders in code blocks; remove if the varnish theme handles this natively -->
<script>(function(){var CSS='.sh-placeholder{color:#c2410c;font-weight:700}[data-bs-theme="dark"] .sh-placeholder,html.dark .sh-placeholder{color:#fdba74}@media (prefers-color-scheme: dark){[data-bs-theme="auto"] .sh-placeholder{color:#fdba74}}';var RX=/<labname>|<myusername>/g;function firstMatch(el){var w=document.createTreeWalker(el,NodeFilter.SHOW_TEXT,null),nodes=[],full='';while(w.nextNode()){nodes.push({n:w.currentNode,s:full.length});full+=w.currentNode.nodeValue;}RX.lastIndex=0;var m;while((m=RX.exec(full))){var s=m.index,e=s+m[0].length,inSpan=false,parts=[];for(var j=0;j<nodes.length;j++){var ns=nodes[j].s,ne=ns+nodes[j].n.nodeValue.length;if(ne<=s||ns>=e)continue;parts.push({node:nodes[j].n,a:Math.max(s-ns,0),b:Math.min(e-ns,nodes[j].n.nodeValue.length)});var p=nodes[j].n.parentNode;while(p&&p!==el){if(p.classList&&p.classList.contains('sh-placeholder')){inSpan=true;break;}p=p.parentNode;}}if(!inSpan&&parts.length)return parts;}return null;}function wrapParts(parts){for(var i=parts.length-1;i>=0;i--){var t=parts[i].node,txt=t.nodeValue,a=parts[i].a,b=parts[i].b;var span=document.createElement('span');span.className='sh-placeholder';span.textContent=txt.slice(a,b);var f=document.createDocumentFragment();if(a>0)f.appendChild(document.createTextNode(txt.slice(0,a)));f.appendChild(span);if(b<txt.length)f.appendChild(document.createTextNode(txt.slice(b)));t.parentNode.replaceChild(f,t);}}function run(){var st=document.createElement('style');st.textContent=CSS;document.head.appendChild(st);document.querySelectorAll('pre,code').forEach(function(el){var guard=0,parts;while((parts=firstMatch(el))&&guard++<500){wrapParts(parts);}});}if(document.readyState==='loading'){document.addEventListener('DOMContentLoaded',run);}else{run();}})();</script>
