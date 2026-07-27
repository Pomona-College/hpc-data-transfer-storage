---
title: "Data Organization Best Practices"
teaching: 20
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- How should I organize my data on Sagehen?
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
/rhome/username/
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

/bigdata/labname/
+-- group_projects/            (collaborative work)
+-- lab_resources/             (shared reference data)
```

## Organizational Principles

**Separate data types**: Keep raw data separate from processed data and results. This enables clean, reproducible pipelines.

**Use meaningful names**: `dataset_v1.csv` is better than `data.csv`. Include version numbers or dates: `results_2024_03_05.txt`.

**Include metadata**: Add a `README.md` in each project explaining the structure, data sources, and how to reproduce the analysis.

**Archive completed projects**: Compress inactive projects with `tar czf project_2023.tar.gz project_2023/` and move to `/bigdata/labname/archive/`.

## File Permissions for Collaboration

When sharing files through `/bigdata/labname`, set appropriate permissions:

```bash
# Make files readable by lab members
chmod 644 /bigdata/labname/shared_file.txt

# Make directories traversable
chmod 755 /bigdata/labname/shared_dir/

# Set default permissions for future files
umask 0022  # Add to ~/.bashrc
```

## Sharing Data

### With Lab Members (Internal)

```bash
mkdir -p /bigdata/labname/shared_results
cp analysis_output.csv /bigdata/labname/shared_results/
chmod 644 /bigdata/labname/shared_results/analysis_output.csv
```

Notify collaborators: "Results available in `/bigdata/labname/shared_results/`"

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
ln -s /bigdata/labname/reference_data ~/reference
```

Find potential duplicates:

```bash
find /rhome/username -name "*.bak" -o -name "*_old" -o -name "*~"
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
- Use `/bigdata/labname` for collaborative data with appropriate permissions
- Use symbolic links instead of copies for shared reference data

::::::::::::::::::::::::::::::::::::::::::::::::::
