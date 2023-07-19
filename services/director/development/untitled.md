---
description: Director Development using Vagrant
---

# Vagrant Setup

## Getting Started

Similar to Ion, Director has a setup using Vagrant. This page will go over about setting up for Director using Vagrant.

First off, install [VirtualBox](https://www.virtualbox.org/wiki/Downloads) and [Vagrant](https://www.vagrantup.com/downloads), VirtualBox is a virtualization program, and Vagrant is the development environment in where we'll run Director. For more information about those two services, please take a look at their documentation linked below.

If it is not installed already, install [Git](https://git-scm.com/). Git is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency. This is essential for Director if you are going to be pushing changes to it.

Ensure you have an SSH key set up with GitHub by running `ssh -T git@github.com`. You should be greeted by your username. If not, set up an SSH key with GitHub by following [these instructions](https://help.github.com/articles/generating-an-ssh-key/).

Clone the Director 4.0 repository onto your computer and `cd` into the new directory. Essentially just run `git clone git@github.com:tjcsl/director4.git director && cd director`.

{% hint style="info" %}
Note: if your host machine is running Windows, please run `git config core.autocrlf input` before cloning to prevent line ending issues.
{% endhint %}

```bash
$ git clone git@github.com:tjcsl/director4.git director
$ cd director
```

Once inside the `director` directory, run `vagrant plugin install vagrant-vbguest`. If you are on Windows, also run `vagrant plugin install vagrant-winnfsd`.

Run `vagrant up && vagrant reload` and wait while the development environment is set up. This will download a Vagrant image and provision the resulting VM.

## Post-Setup

After successfully setting up the Vagrant environment, you will want to actually access your sandbox.

Start by connecting to the Vagrant box using `vagrant ssh` to connect to the VM.

Once inside, run `cd director` to change into the repo and `./scripts/install_dependencies.sh` to install Director's Python dependencies using `pipenv`.

Once completed, you may now work on Director `scripts/start-servers.sh` will open a `tmux` session with the four servers each running in a separate pane.

* Note: If you are not familiar with `tmux`, we recommend [https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/) and [https://tmuxcheatsheet.com/](https://tmuxcheatsheet.com/) as starting resources.
* See [this](https://github.com/tjcsl/director4/blob/master/docs/docs/tmux.md) for an explanation of the components of the development `tmux`

When you are finished, type `exit` to exit the VM and `vagrant halt` to stop it. When you want to work on Director 4.0 again, `cd` into this directory, run `vagrant up` and `vagrant ssh` to launch the VM and connect to it, and then run `exit` and `vagrant halt` to exit and shut it down.

## Documentation

* VirtualBox - [https://www.virtualbox.org/manual/UserManual.html](https://www.virtualbox.org/manual/UserManual.html)
* Vagrant - [https://www.vagrantup.com/docs](https://www.vagrantup.com/docs)
* Git - [https://git-scm.com/docs](https://git-scm.com/docs)
