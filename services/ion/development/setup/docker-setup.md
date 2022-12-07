# Docker Setup

## Setting up Docker

Docker is one of the two options you have for managing Ion's development environment. Docker is software platform that used to build applications using containers. If you want to learn more about Docker, refer to its [documentation](https://docs.docker.com/).

First, download and install Git from [here](https://git-scm.com/downloads) if you are on Windows. Ensure you have an SSH key set up with GitHub by running `ssh -T git@github.com`. You should be greeted by your username. If not, set up an SSH key with GitHub by following [these instructions](https://help.github.com/articles/generating-an-ssh-key/).

Install Docker by following [these instructions](https://www.docker.com/products/docker-desktop/) based on your operating system. For Windows and Mac users, install Docker Desktop.

After installing Git and Docker, fork the [`tjcsl/ion`](https://github.com/tjcsl/ion) repository, and clone your forked Ion repository_._ Once the cloning completed, `cd` into the `intranet` directory.

```
$ git clone git@github.com:<YOUR_GITHUB_USERNAME>/ion.git intranet
$ cd intranet
```

Note: if your host machine is running Windows, please run `git config core.autocrlf input` before cloning to prevent line ending issues.

`cd` into the `config/docker` directory and edit the `make_admin.py` file, replacing the `<YOUR_USERNAME>` with your Ion username.

Save the file and run `cd config/docker`. Once done, run `docker-compose up -d` to compose the container.

### Post Setup

Navigate to [http://localhost:8080](http://localhost:8080/) in the web browser of your choice. If this is the first time composing the container (in which in this case; running `docker-compose up -d` for the first time on that computer), you may have to wait for approximately 60 seconds for it to start running. When presented with the login page, log in with your Ion username and the password `notfish`.

### Useful Commands

#### Interacting with the application:

If you need to run a Django command like `makemigrations`, `collectstatic` or `shell_plus`, run `docker exec -it intranet bash` in your terminal. That will give you a shell into the application container. You can also use this to run scripts like `build_sources.sh`. If you need to view the output from or restart `runserver`, run `docker attach application`.

### History

The use of Docker for developing Ion started in early 2022 when then-understudy Justin Lee (`2025jlee`) developed way to make a Ion Development Environment using Docker. The reason for a Docker environment was to be a quicker and easier way to develop on Ion, rather than the old and slow environment of Vagrant. If you are developing for Ion, it is recommended to start with Docker as this might be the primary setup for the Ion environment.
