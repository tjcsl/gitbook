# Ansible

**Ansible** is an automation tool that allows us to run scripts to configure services and devices across the lab. Our goal with Ansible is to have the ability to bring up every service by only running the ansible plays.

For the purposes of the Lab, "ansibilize" is a word that refers to making a set of systems operate under the control of Ansible plays completely.

To see more about how to use ansible, refer to this link: [https://docs.ansible.com/](https://docs.ansible.com/)

## Organization

In order to effectively use Ansible, you need to know it's vocabulary.

* **Hosts** are machines that Ansible can manage
* **Groups** are sets of hosts defined in the `hosts` file
* **Modules** contain a single thing for Ansible to do. There is a `file` module for doing file operations, an `apt` module for installing packages, a `shell` module for running shell commands, etc. All modules can be found at [https://docs.ansible.com/ansible/latest/modules/modules\_by\_category.html](https://docs.ansible.com/ansible/latest/modules/modules_by_category.html).
* **Tasks** are a set of Modules to be run in order. Tasks can also include tasks from other files.
* **Roles** are assigned to Groups and contain a single entrypoint task at `roles/<role_name>/tasks/main.yml`. Each role can also contain its own files at `roles/<role_name>/files`, and ditto for templates.
* **Plays** define a set of Roles to apply to a set of Groups. Usually it's a many Role to one Group relationship per play.

All Ansible configuration files are written in [YAML](https://yaml.org/). The best way to learn YAML is by reading existing YAML files. It's structured a bit like JSON, only indentation based like Python. So instead of

```javascript
{"dictionary-key": ["list-value1", "list-value2"]}
```

you have

```yaml
dictionary-key:
  - list-value1
  - list-value2
```

## CSL's Role Management

There are two roles that should be applied to almost every machine: `common` and `auth`. `common` installs a good suite of common utilities, and `auth` allows the machine to have [Kerberos](../authentication/kerberos.md) login. Other than that, we usually assign our roles on a per-group basis, although that isn't too modular. [AFS](../storage/afs/) support has its own role, but unfortunately [CephFS](../storage/cephfs.md) doesn't yet.

## Running Ansible

First, you will want to install Ansible either by using your OS's package manager or `pip install ansible`.

Then, you should `cd` to the place you have cloned your Ansible repository, and run the following command:

```bash
ansible-playbook -i hosts --ask-vault-pass <target_play>.yml
```

{% hint style="warning" %}
Before ansible can be run on a remote host, the remote host must have python installed.
{% endhint %}

You will usually need access to the `ansible_vault` password in [Passcard](../authentication/passcard/) before you can run any important plays, as a module in `auth` needs to access an encrypted file. For more `ansible-playbook` options, you should run `ansible-playbook --help`.

### Advanced usage

The Ansible plays for running the [Cluster](../../services/cluster/) are very complicated and take advantage of some cool things Ansible has to offer.

The first thing is the `when` directive, added to the end of any module, specifying when a module should be run. Just like in `common` and `auth`, different package managers with different packages names are used on different systems, but by only running `apt` when on [Ubuntu](../servers/ubuntu-server.md) and `yum` when on [CentOS](../servers/centos.md), we can account for that.

The other thing is the `--extra_vars` flag the `ansible-playbook` command. You can pass in a variable to make the play do a fresh install of all built-from-source packages, uninstall a previous version of a package, and I would say more but that's about it. You should read the play to figure out how that works.

## Trivia

* The Ansible is a fictional FTL communication device appearing in the _Ender's Game_ series, used by aliens to manage their entire fleet

