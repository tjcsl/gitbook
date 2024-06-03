# SSH Passwordless Login

**SSH Passwordless Login** is set up on all CSL machines in order to seamlessly SSH into different machines without having to type in your password every time. This is achieved using user SSH keys.

In order to set up passwordless login, run `ssh-keygen` and hit enter through the setup wizard. This will create two files, `id_rsa` and `id_rsa.pub`, in `[YOUR HOME DIRECTORY]/.ssh`. Log into a remote access server, create the `.ssh` folder in there (i.e., `mkdir ~/.ssh`), and upload the two files in there. Run `chmod 600 ~/.ssh/id_rsa`; this fixes permission issues. When you're at school, visit [https://ipa.tjhsst.edu](https://ipa.tjhsst.edu) and login with your CSL username and password. Go to your profile page. Click on "Add" next to "SSH Public Keys" and upload the contents of `id_rsa.pub`. You should now be able to SSH without a password to any machine that you have authorization for.
