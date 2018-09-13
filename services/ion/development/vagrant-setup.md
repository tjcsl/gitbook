# Vagrant Setup

## Setting up Vagrant

Vagrant is used to manage Ion's development environment so that it closely resembles the production environment. To get started, download and install git from [here](https://git-scm.com/downloads) if you are running Windows or git is not installed.  After that, download and install Virtualbox from [here](https://www.virtualbox.org/wiki/Downloads) and Vagrant from [here](http://docs.vagrantup.com/v2/installation/index.html).  When you're installing Vagrant, you should install it on your host OS.

Ensure you have an SSH key set up with GitHub by running `ssh -T git@github.com`. You should be greeted by your username. If not, set up an SSH key with GitHub by following [these instructions](https://help.github.com/articles/generating-an-ssh-key/).

With Vagrant and Virtualbox installed, clone the Ion repository onto the host computer and `cd` into the new directory.

Note: if your host machine is running Windows, please run `git config core.autocrlf input` before cloning to prevent line ending issues.

```text
$ git clone git@github.com:tjcsl/ion.git intranet
$ cd intranet
```

In the `config` directory, copy the file `devconfig.json.sample` to `devconfig.json` and edit the properties in `devconfig.json` as appropriate. Ensure `ssh_key` is set to the same SSH key registered with GitHub \(e.g. `id_rsa`\). 

{% hint style="info" %}
The other values specified in `devconfig.json` are optional.  The `ldap_simple_bind_password` is not needed and is a remnant of the old LDAP-based authentication scheme.
{% endhint %}

Run `vagrant plugin install vagrant-vbguest vagrant-bindfs` If you are on Windows, also run `vagrant plugin install vagrant-winnfsd`

The `Vagrantfile` should be edited to prevent connecting to the VPN.  `setup_host` on line 38 should be commented and line 70 should be set to `config.vm.synced_folder ".", "/vagrant-nfs", type: :nfs, nfs_udp: false`.

{% hint style="info" %}
Connecting to the CSL VPN may be necessary to test Kerberos authentication or other functionality that requires connection to CSL services.  In that case, `setup_host` should remain noncommented.
{% endhint %}

Run `vagrant up && vagrant reload` and wait while the development environment is set up. When asked to select a network interface for bridging, enter the number corresponding to one that is active. To automatically select this interface in the future, set the "network\_interface" key in `devconfig.json` to the name of the interface you selected \(e.g. `"en0: Wi-Fi (AirPort)"`\). There may be repeated warnings similar to "`Remote connection disconnect` on the second `vagrant up`. After several minutes they will stop. Once the provisioning process is complete, run `vagrant ssh` to log in to the development box.

Move into the `intranet` directory and run `workon ion` to load the Python dependencies. `workon ion` should always be the first thing you run after you SSH into the development box.

The Git repository on the host computer is synced with `~/intranet` on the virtual machine, so you can edit files within the repo on the host computer with a text editor of your choice and the changes will be immediately reflected on the virtual machine.

### Troubleshooting

If you get a `SIOCADDRT: Network is unreachable` error when running `vagrant up`, you need to start the OpenVPN client.

If you see a `Adding routes to host computer...` message, you probably forgot to start the OpenVPN client.

## Post-Setup

After successfully setting up the Vagrant environment, you will want to actually access your sandbox.

Start by connecting to the Vagrant box using `vagrant ssh`. \(Consider running all of the following in a `screen` or `tmux` session.\) Make sure you’re in the `intranet` directory, and run `python manage.py migrate`. This will set up the Postgres database.

You can then start the built-in Django web server with `fab runserver`. Now that you are running the development server, open a browser to [http://127.0.0.1:8080](http://127.0.0.1:8080) and log in. If it fails, check the output of `manage.py runserver`.

### Setting Up Groups

Currently, there are no default groups set up when you first install Ion. In order to grant yourself administrative privileges, you must be a member of the `admin_all` group.

To create and add yourself to the global administrator group, run the following commands:

```python
$ ./manage.py shell_plus
Python 3.5.2 (default, Nov 23 2017, 16:37:01) 
Type 'copyright', 'credits' or 'license' for more information
IPython 6.2.1 -- An enhanced Interactive Python. Type '?' for help.
>>> user = User.objects.get_or_create(username="YOURUSERNAME")[0]
>>> group = Group.objects.get_or_create(name="admin_all")[0]
>>> user.groups.add(group)
>>> user.is_superuser = True
>>> user.save()
```

### Connecting and Disconnecting from the VM

When you want to close the VM environment, make sure you have exited out of the ssh session and then run `vagrant suspend`. To resume the session, run `vagrant resume`. Suspending and resuming is significantly faster than halting and starting, and also dumps the contents of the machine’s RAM to disk.

### Setting up Files

You can find a list of file systems at `intranet/apps/files/models.py`. To add these systems so that they appear on the Files page, run the statements found in the file. A sample is shown below:

```python
$ ./manage.py shell_plus
Python 3.5.2 (default, Nov 23 2017, 16:37:01) 
Type 'copyright', 'credits' or 'license' for more information
IPython 6.2.1 -- An enhanced Interactive Python. Type '?' for help.
>>> Host.objects.create(name="Computer Systems Lab", code="csl", address="remote.tjhsst.edu", linux=True)
```

### Increasing RAM

With any RAM lower than the default 2048MB, you may run into performance constraints. If you encounter signifigant issues, it is recommended to bump the VM’s amount of memory, through VirtualBox Manager, to at least that amount.

