# Zoidberg

## What is Zoidberg

Zoidberg is a very powerful server with **4 NVIDIA Telsa K80** GPUs. It is part of the HPC Cluster in name and location only. It's treated as mostly separate, and can be used outside of [Slurm](using-infoprism.md). Zoidberg is by far the most powerful HPC node as well, potentially the most powerful machine in the entire CSL.

It can be used to run large machine learning projects that require a lot of power.

## Accessing Zoidberg

First, ssh into TJ's remote access server, using your TJ Username (e.g. 2021jdoe)

```
ssh tj-username@remote.tjhsst.edu
```

Afterwards, ssh into zoidberg.

```
ssh zoidberg
```

## Conda

To run machine learning projects with python, it is recommended to set up virtual environments for each project (so each project can use different versions of python packages if needed).&#x20;

To achieve this, it is best to use Conda on the Zoidberg environment.

Conda is a package management tool that is pretty powerful. If you want to learn more about it, here are the docs for [Conda](https://docs.conda.io/projects/conda/en/latest/).

## How to use Conda

### Creating a new environment

```
conda create -n VENV_NAME python
```

### Activating a new environment

```
conda activate VENV_NAME
```

### Deactivating a new environment

```
conda deactivate
```

### Installing a package

```
conda install PACKAGE_NAME
```



