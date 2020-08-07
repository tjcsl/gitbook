# Environment

Now that you have set up your development environment, you should get familiar with your environment and the Ion code base.

## Useful Commands

### Vagrant

To manage your Vagrant box, you should use the following commands:

| Command | Description |
| :--- | :--- |


| `vagrant suspend` | Saves the state of the VM |
| :--- | :--- |


| `vagrant resume` | Resumes the previous state of theVM |
| :--- | :--- |


| `vagrant ssh` | SSHs into the VM |
| :--- | :--- |


| `vagrant reload` | Halts the VM and then brings it back up |
| :--- | :--- |


| `vagrant up` | Brings up the VM according to specified `Vagrantfile` |
| :--- | :--- |


<table>
  <thead>
    <tr>
      <th style="text-align:left"><code>vagrant destroy</code>
      </th>
      <th style="text-align:left">
        <p>(A DANGEROUS COMMAND)</p>
        <p>Stops the VM &amp; permanently destroys the VM and its contents</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>

To manage the Vagrant box within the box, you should use the following commands:

| Command | Description |
| :--- | :--- |
| `workon ion` | Initialize virtualenvwrapper |
| `fab runserver` | Run a server in development |
| `./manage.py migrate` | Migrate the database if not already |
| `./manage.py shell_plus` | Enter a Python shell |
| `./manage.py collectstatic` | Collect static files into a directory |

You should ALWAYS make sure `workon ion` has been run after SSH-ing into the VM. The database should be migrated fairly regularly \(especially after changes in the database/models\) in development.

Running `fab runserver` should make the server accessible from `localhost:8080` \(on your host machine's web browser\).

Running `./setup.py test` should test your current code base against the Ion test suite.

## Code base

## Overall Structure

### Root

The root of the Ion git repository is split into many parts in order to allow developers to access information quickly:

* `config`:  Contains scripts/files used provision the Vagrant box
* `cron`: Contains cron bash scripts that are only run on production
* `docs`: Contains old Ion docs
* `intranet`: Contains most of Ion's code
* `Ion.egg-info`: Contains information about the Python Ion eggs
* `migrations`: Skeleton directory only created for migrations
* `scripts`: Contains scripts useful for Ion developers

Some useful files in the root of the Ion git repository include:

* `COPYING`: Contains the GPLv2+ for the Ion code
* `fabfile.py`: Describes behavior of `fab`.  Used for development/deployment.
* `manage.py`:  Contains wrapper for Django shell management commands
* `README.rst`: Contains the README
* `requirements.txt`: Contains Ion's dependencies
* `setup.py`: Describes the Python Ion package
* `Vagrantfile`: Describes configuration of the Vagrant box

## Intranet

Some useful sub directories of the `intranet` directory include \(as per Django best practices\):

* `apps`: Contains the vast majority of Django apps
* `middleware`: Contains Django middleware
* `settings`: Contains Django settings for Ion
* `static`: Contains CSS, images, JS, SVGs, themes, and useful documents
* `templates`: Contains Django templates and email templates
* `test`: Contains the Ion base test suite
* `utils`: Contains side-wide utilities

All other files in this directory are per Django convention. If you do not know what they do, Google it.

Within the `apps` directory, there are multiple Django apps with descriptive names.

