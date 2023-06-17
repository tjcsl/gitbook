# Docker Setup

## Setting up Docker

Docker is one of the two options you have for managing Ion's development environment. Docker is a software platform that is used to build applications using containers. If you want to learn more about Docker, refer to its [documentation](https://docs.docker.com/).

First, download and install Git from [here](https://git-scm.com/downloads) if you are on Windows. Ensure you have an SSH key set up with GitHub by running `ssh -T git@github.com`. You should be greeted by your username. If not, set up an SSH key with GitHub by following [these instructions](https://help.github.com/articles/generating-an-ssh-key/).

Install Docker by following [these instructions](https://www.docker.com/products/docker-desktop/) based on your operating system. For Windows and Mac users, install Docker Desktop.

After installing Git and Docker, fork the [`tjcsl/ion`](https://github.com/tjcsl/ion) repository, and clone your forked Ion repository_._ Once the cloning completed, `cd` into the `intranet` directory.

```
$ git clone git@github.com:<YOUR_GITHUB_USERNAME>/ion.git intranet
$ cd intranet
```

{% hint style="info" %}
Note: if your host machine is running Windows, please run `git config core.autocrlf input` before cloning to prevent line ending issues.
{% endhint %}

Also make sure that Docker Compose is installed along with `docker`, this is important when it comes to building and bringing the development environment to life.

Run `docker compose build` to get all the dependences that Ion needs, like `redis`, `postgres`, `celery`, etc. This should take a minute or two.

Once after the building is complete, run `docker compose up` to compose the container. You can add `-d` flag to the command if you wish.

### Post Setup

Navigate to [http://localhost:8080](http://localhost:8080/) in the web browser of your choice. If this is the first time composing the container (in which in this case; running `docker-compose up -d` for the first time on that computer), you may have to wait for approximately 60 seconds for it to start running. When presented with the login page, you can login in the default admin account which is `admin`/`notfish`.

### Useful Commands

#### Interacting with the application:

If you need to run a Django command like `makemigrations`, `collectstatic` or `shell_plus`, run `docker exec -it intranet bash` in your terminal. That will give you a shell into the application container. You can also use this to run scripts like `build_sources.sh`. If you need to view the output from or restart `runserver`, run `docker attach application`.

### History

The use of Docker for developing Ion was anticipated for a long time after Ion was first release in 2015. The former Vagrant environment didn't really had a multitude of problems, but the idea of making a VM and configuring the VM was a bit annoying for many Sysadmins and understudies who wanted to build the development environment. The Development of the new environment started in early 2022 when then-understudy Justin Lee (`2025jlee`) developed way to make a Ion Development Environment using Docker. The Docker environment solved some of the problems that Vagrant had, most notably the speed building the environment.

Around early 2023, the Docker environment went through a second round of updating to make the environment more quicker and easier to build.
