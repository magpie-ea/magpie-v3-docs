# Installation

You can install magpie on a server for online use or locally on your machine.

The latest version of magpie-base (v3.x) requires magpie-backend v2.x to run on the server to function properly. Previous iterations
of magpie, require magpie-backend v1.x but will usually also run with magpie-backend v2.x.

## Installation on Heroku

The magpie server app can be hosted on any hosting service or your own server. The process for
installation on [Heroku](https://www.heroku.com/) is described [in the README of the backend project](https://github.com/magpie-ea/magpie-backend#deployment-with-heroku).

## Installation using docker

If you are already familiar with [docker and docker-compose](https://docs.docker.com/), the magpie backend is also available
as a ready-to-install docker container. You can find more details about this on the GitHub repository: <https://github.com/magpie-ea/magpie-docker/>

## Installation locally

The first-time installation requires an internet connection. After it is finished, the server can be launched offline.

(Note that for local deployment, the default username is `default` and the default password is `password`. You can change them in `config/dev.exs`.)

### First time installation

1. Install Docker from https://docs.docker.com/install/. You may have to launch the application once in order to let it install its command line tools. Ensure that it's running by typing `docker version` in a terminal (e.g., the Terminal app on MacOS or cmd.exe on Windows).

   Note:

   - Although the Docker app on Windows and Mac asks for login credentials to Docker Hub, they are not needed for local deployment . You can proceed without creating any Docker account/logging in.
   - Linux users would need to install `docker-compose` separately. See relevant instructions at https://docs.docker.com/compose/install/.

2. Ensure you have [Git](https://git-scm.com/downloads) installed. Clone the server repo with `git clone https://github.com/magpie-ea/magpie-backend.git` or `git clone git@github.com:magpie-ea/magpie-backend.git`.

3. Open a terminal (e.g., the Terminal app on MacOS or cmd.exe on Windows), `cd` into the project directory just cloned via git.

4. Run `docker-compose up` to launch the application every time you want to run the server. Wait until the line `web_1 | [info] Running MAGPIE.Endpoint with Cowboy using http://0.0.0.0:4000` appears in the terminal.

5. Visit `localhost:4000` in your browser. You should see the server up and running.

   Note: Windows 7 users who installed _Docker Machine_ might need to find out the IP address used by `docker-machine` instead of `localhost`. See [Docker documentation](https://docs.docker.com/get-started/part2/#build-the-app) for details.
