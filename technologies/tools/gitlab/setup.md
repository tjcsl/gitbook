# Setup

## Setting up GitLab

### Installing GitLab

#### Ansible Installation

After running Ansible, note that the current play does not contain settings for SMTP. To set up SMTP, in `/etc/gitlab/gitlab.rb`, find the following configuration settings, uncomment the lines and set their values to the ones below:

```text
gitlab_rails['smtp_enable'] = true
gitlab_rails['smtp_address'] = "mail.tjhsst.edu"
gitlab_rails['smtp_port'] = 22
```

After saving these settings to `gitlab.rb`, apply these settings using `gitlab-ctl reconfigure`.

#### Manual Installation

{% hint style="danger" %}
In this method, **never** execute `gitlab-ctl reconfigure` unless specifically stated to.
{% endhint %}

Manual setup can be found on GitLab's website [here](https://about.gitlab.com/installation/). However, the only commands you need to run successively are as follows:

```text
sudo apt-get update
sudo apt-get install -y curl openssh-server ca-certificates
sudo apt-get install -y postfix
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.deb.sh | sudo bash
sudo EXTERNAL_URL="http://gitlab.tjhsst.edu" apt-get install gitlab-ee
```

After running these commands, head to `/etc/gitlab/`

1. Locate `gitlab.rb` in the ansible repository under `roles/gitlab/files`
2. In `/etc/gitlab` execute`sudo mv gitlab.rb gitlab-OLD.rb`
3. Copy the Ansible version of `gitlab.rb` into `/etc/gitlab`.

### Initial Configuration

{% hint style="warning" %}
Make sure you ran `gitlab-ctl reconfigure` after editing `gitlab.rb`as instructed in previous steps to apply all changes. This usually takes 30 seconds to 3 minutes depending on the settings.
{% endhint %}

#### Initial Login

After initial setup, log in using the Standard option as `root`, with the default GitLab password. Immediately change this to a more secure password in line with CSL standards.

#### Removing Standard and Sign-Up options

On initial login, note how there were three possible options: LDAP, Standard, and Sign-Up. As we do not use the Standard and Sign-Up options, it is necessary to remove them by following the steps below:

1. Head to the Admin Options, an icon depicting a wrench links here
2. On the left sidebar, find the Settings option and open the page.
3. Locate the respective check boxes under the Sign-up and Sign-in drop-downs.

{% hint style="info" %}
The exact naming of the drop-downs and check-boxes are omitted as they have historically changed with versions.
{% endhint %}

{% hint style="success" %}
When Standard and Sign-Up are missing from the login page leaving LDAP, configuration is done.
{% endhint %}

