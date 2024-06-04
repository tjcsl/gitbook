---
description: This document describes how to use TJ's JupyterHub instance
---

# JupyterHub

## What is JupyterHub?

[JupyterHub](https://jupyter.org/hub) is a fully-online IDE that is very similar to Google's Colab interface. JupyterHub's uses vary from simple text file editing to large IPython notebooks. This document will mainly focus on the advanced features of JupyterHub since the more simple features are easily discoverable through the interface and are fairly self-explanatory.

## How do I access JupyterHub?

TJ's JupyterHub instance is available at[ https://jupyterhub.tjhsst.edu](https://jupyterhub.tjhsst.edu). You will be redirected to Ion to authenticate. Then, you will be presented with options for your Jupyterlab instance:

* Default (4 CPUs, 8GB RAM)
* Compute (8 CPUs, 16GB RAM)
* 1 GPU (8 CPUs, 16 GB RAM, 1 GPU)
* 2 GPUs (8 CPUs, 16 GB RAM, 2 GPUs)
* 4 GPUs (8 CPUs, 16 GB RAM, 4 GPUs)

Please only choose what you need. Depending on usage, it may take longer to spawn instances for higher resource allocations, and others may be prevented from using resources allocated to you if you are not using them.

You will then be placed in a queue to wait for your own server to spawn. Our instance is configured such that your server may be spawned on one of 53 different machines. Even though your server is spawned on random machines, all your files are stored in a common location:  `/cluster/<username>`.&#x20;

## Advanced Features

### Python Interfaces

After logging in you may be greeted with a "Launcher" tab. This tab will have multiple options, including:

* Python Notebook
* Python Console
* Terminal

The Python Notebook and Python Console are two separate interfaces, and you may experiment with either of them.&#x20;

![As you see in the top two rows, the options "Notebook" and "Console" are available. Notice how they are both named "Python (default)"](../../.gitbook/assets/launcher.png)

Both of these Python interfaces are based in the "default" Python environment, meaning they have the default packages. In most cases the default packages will suffice, but if you need to install custom packages read the instructions below:

### Creating Conda Environments

Both the Python Notebook and Console are backed by [Conda](https://docs.conda.io/en/latest/) environments. Familiarize yourself with Conda environments before reading on.&#x20;

To create a personal custom Conda environment run the following:

On `infocube` or any cluster node run the following in your home dir (`/cluster/<your_csl_username>`)

```bash
conda create --name my_personal_environment ipykernel
```

The `--name` parameter can be anything you wish (in this case `my_personal_environment`). Additionally,`ipykernel` is **REQUIRED** at the end.

After creating the Conda environment, activate it:

```bash
conda activate my_personal_environment
```

Replace `my_personal_environment` with the environment name you previously chose.

Finally add the environment to JupyterHub:

```bash
python -m ipykernel install --user --name "my_env" --display-name "My Env"
```

The `--name` and `--display-name` parameters can be anything you wish.

After running the previous commands your launcher should now look like this:

![Notice how a second option for both Notebook and Console is now available. The names match the "--display-name" parameter in the previous step.](../../.gitbook/assets/new\_launcher.png)

Now you have created a personal environment you have total control of. You may use `conda` to install any custom packages you wish. Be sure to click on your new Python Notebook/Console option (not the default option) if you wish to access your custom environment.

### Removing Environments

{% hint style="danger" %}
Removing environments is a permanent action and cannot be reversed!
{% endhint %}

Removing the Conda environment is simple:

```bash
conda env remove -n my_personal_environment
```

Removing the entry is a little more involved:

```bash
rm -r /cluster/<username>/.local/share/jupyter/kernels/<name>
```

* The `<name>` parameter must match the `--name` parameter passed to the `python -m ipykernel ...` step, **NOT --display-name**! You may also `ls` around in this directory to find your matching environment and then `rm -r` the directory
* After removing the directory, reload JupyterHub and the entry will now be removed.
