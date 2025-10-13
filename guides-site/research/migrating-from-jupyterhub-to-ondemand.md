---
description: This page details the changes between JupyterHub and Open OnDemand
---

# Migrating from JupyterHub to OnDemand

Open OnDemand is the replacement for JupyterHub - see [ondemand](ondemand/ "mention")for information on how to use it. This page details major changes that may need to be taken into account during the migration.

{% hint style="warning" %}
As of October 2025, JupyterHub is currently deprecated, and is planned to be removed in January 2026.

For any questions, or to request an extension, please email [sysadmins@tjhsst.edu](mailto:sysadmins@tjhsst.edu).
{% endhint %}

## Packages Upgrades

The first major change is the Ubuntu version upgrades - the nodes are now running Ubuntu 24.04! This does mean that any virtual environments or global installations using the global python from Ubuntu 20.04 or 22.04 will now be broken, and have to be recreated. Additionally, it does imply upgrades to versions of packages like `CMake` and `OpenCV` which may require small tweaks in lesson plans or handouts.

Our recommendation for recreating these environments is to use `pixi`, so that future Ubuntu upgrades will not break the environment. In order to create a `pixi` environment, run the following commands:

```bash
cd ~/your-environment-directory
pixi init
pixi add "python==3.13" # or whatever version of python you want to pin to
pixi add "name of your package" "name of your other package"
```

Note that by default, `pixi` uses `conda-forge` to install dependencies. To install packages from [PyPI](https://pypi.org/), use the `--pypi` flag when running `pixi add`.

Additionally, the default environments for Physics, Machine Learning, Octave, and Quantum Computing have been upgraded to use newer versions of their packages. If you encounter a package missing that existed on JupyterHub, please contact [sysadmins@tjhsst.edu](mailto:sysadmins@tjhsst.edu).

## No More Global Packages

### Removal of Conda Environments

Conda has been phased out of Jupyter, and instead replaced with [Pixi](https://pixi.sh/). As such, the old commands like `conda activate physics` will no longer work. Instead, they have been replaced with the environments mentioned in the next section.

### Lmod Environments

JupyterHub had many packages that were installed globally, and as such could be run in the terminal without activating any environment. Starting with Ubuntu 24.04, this is no longer the case, and users must activate an environment to use any specific packages. We use [Lmod](https://lmod.readthedocs.io/en/latest/010_user.html) to manage environments. In the following, a brief overview of the commands are listed.

To see a list of available environments, run

```bash
module avail
```

For more information on a specific module, run either of these commands:

```bash
module whatis "name of module, e.g. physics"
module show "name of module, e.g. ml"
```

To load an environment - taking the "physics" environment as an example, run:

```bash
module load physics
```

After activating a module, the packages inside `physics` will be accessible (for example, try running `python3 -c "import matplotlib"`).

To deactivate the environment, the command is:

```bash
module unload physics
```

