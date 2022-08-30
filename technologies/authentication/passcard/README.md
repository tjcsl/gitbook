# Passcard

The CSL passcard contains the root passwords for all CSL systems. The passcard is maintained in a git repository hosted on GitLab. The repository is maintained by the lead Sysadmins and any questions, concerns or bug reports should be directed to them.

### Requirements

To use the passcard system, you must have the following:

* &#x20;Git
* &#x20;GPG
* &#x20;A GPG key
  * &#x20;If you are using the official CSL passcard, the public key should be signed by a lead Sysadmin or by someone with all the lead Sysadmins in their trustnet and also stored on a public keyserver.
* &#x20;Python3
  * &#x20;To use the wrapper script

### Accessing the Passcard

The git repository is accessible via GitLab at [gitlab.tjhsst.edu/sysadmins/passcard](https://gitlab.tjhsst.edu/sysadmins/passcard). To clone the repository, run the following command: `git clone git@gitlab.tjhsst.edu:sysadmins/passcard.git`. You can clone it from outside the TJ network, but to access the passcard repository you MUST have a GitLab account.

### Using the Passcard

The passcard git repository has a wrapper script (passcard.py) along with GPG encrypted passwords individually encrypted in the passwords folder. Since the passwords are individually encrypted, each password is encrypted with the keys of the people who should have access to it. For example, somebody can have access to [Core0](../../../machines/switches/core0.md)'s password without having access to [Waitaha](../../../machines/ceph/waitaha.md)'s password. You can use gpg yourself and decrypt these passwords, or you can use the wrapper script which does it all for you.

\
&#x20;For help with the wrapper script, run it without any arguments. Here are the commands you can use:

`./passcard.py get antipodes` will show you the decrypted password for antipodes and antipodes-ilo (to use most commands, you must have gpg installed with your private key imported).

`./passcard.py dump` will make a nice, two-column passcard to stdout of all the passwords you have access to.

`./passcard.py addkey antipodes "Chris Reffett"` will add Chris Reffett's public key to the antipodes passcard so he can then decrypt it.

`./passcard.py add` will give you an interface for adding a new passcard.

Changes to the passcard locally will be pushed automatically by the script to the GitLab repository.

There is also a file `sysadmin_keys.txt` in the repository that contains GPG keys for each of the Sysadmins, as well as the faculty sponsor. Import them at your own risk, but if you wish to import all of them, you can do that with `cat sysadmin_keys.txt | grep -v '#' | grep -Po "0x[0-9A-Za-z]+" | xargs gpg --recv-keys` on a Linux system.
