# Apptainer (HPC) Development Guide (Student-Friendly)

This guide shows how to run the same app workflow on an **HPC cluster** using **Apptainer**
(formerly Singularity). It’s written for **junior app developers**.

Apptainer is common on HPC because it:
- runs containers **without root**
- integrates well with shared filesystems
- works with schedulers like **SLURM**

---

## Big picture

```mermaid
flowchart LR
  A[Edit code] --> B[Build SIF]
  B --> C[5350.sif]
  C --> D[Run on compute node]
  D --> E[App process]
  E --> F[Results / Web UI]
  F --> A
```

---

## What is a `.sif` file?

A `.sif` is an **Apptainer image** (similar to a Docker image, but as a single file).
You can copy it around easily:

- Build: `apptainer build 5350.sif ...`
- Run: `apptainer run 5350.sif`

---

## Typical HPC workflow

### Option 1 (recommended): Build locally → copy to HPC
1. Build on your laptop or a build machine:
   ```bash
   apptainer build 5350.sif Apptainer.def
   ```
2. Copy to the cluster:
   ```bash
   scp 5350.sif youruser@hpc:/path/to/project
   ```
3. Run on the cluster via SLURM.

### Option 2: Build on the cluster (only if allowed)
Some clusters allow building on login nodes or with Apptainer remote build:
```bash
apptainer build --remote 5350.sif Apptainer.def
```

Ask your admins what’s allowed.

---

## Ports on HPC (important!)

On HPC, you usually **cannot** just open `localhost:80` on your laptop.

Instead:
1. Start the app on a **compute node** (via `srun` or `sbatch`)
2. Use **SSH tunneling** from your laptop to that compute node port

```mermaid
flowchart LR
  L[Laptop browser] -->|ssh tunnel| LN[Login node]
  LN -->|forward| CN[Compute node]
  CN -->|port 9080| APP[App in Apptainer]
```

Example tunnel (you edit node name + ports):
```bash
ssh -N -L 8080:localhost:9080 youruser@hpc-login
```
Then open:
- http://localhost:8080

---

## Files provided

| File | Purpose |
|---|---|
| `Apptainer.def` | Definition file (like a Dockerfile for Apptainer) |
| `Makefile.apptainer` | Automated build/run/shell commands |
| `5350.sif` | Built image (generated) |

---

## Using the Makefile (recommended)

First, load Apptainer on the cluster (if needed):
```bash
module load apptainer  # or module load singularity
```

### 1) Show settings
```bash
make -f Makefile.apptainer info
```

### 2) Build the image
If `Apptainer.def` exists:
```bash
make -f Makefile.apptainer build
```

Or build from a Docker image:
```bash
make -f Makefile.apptainer build-from-docker DOCKER_URI=docker://ubuntu:22.04
```

### 3) Run the app
```bash
make -f Makefile.apptainer run
```

### 4) Get a shell
```bash
make -f Makefile.apptainer shell
```

### 5) Run a command inside the container
```bash
make -f Makefile.apptainer exec CMD="python --version"
```

---

## Running with SLURM (example)

Interactive job:
```bash
make -f Makefile.apptainer slurm-run
```

Batch job (example skeleton):
```bash
sbatch --wrap="apptainer run --bind $PWD:/work 5350.sif"
```

Your cluster may require:
- partitions/queues
- time limits
- CPU/memory requests
- GPU flags

---

## Bind mounts (how your code/data gets in)

Apptainer usually runs **read-only** images, and you bring your code/data in via binds.

Example:
- host: current folder
- container: `/work`

```mermaid
flowchart LR
  H[Host: $PWD] -->|--bind $PWD:/work| C[Container: /work]
```

You can add more binds, e.g. scratch:
```bash
make -f Makefile.apptainer run BIND="$PWD:/work,/scratch/$USER:/scratch"
```

---

## Common mistakes and fixes

### “apptainer: command not found”
Load the module:
```bash
module load apptainer
```

### “permission denied” when building
Try remote build (if allowed):
```bash
apptainer build --remote 5350.sif Apptainer.def
```
Or build on your laptop and copy the `.sif` over.

### Web UI not reachable
You probably need SSH tunneling (see Ports section).

---

## Next steps

- Confirm what your cluster supports: Apptainer module, remote build, SLURM policies
- Decide your standard: build locally or remote build
- Add a small “how to run” script for your specific app command/ports

Happy HPC containerizing! 🚀
