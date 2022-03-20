# Installation

You can install magpie on a server for online use or locally on your machine. We explain here how to use Heroku as a hosting service for online use, and how to set up the server app locally. 

## Installation on Heroku

The magpie server app can be hosted on any hosting service or your own server. The process for
installation on [Heroku](https://www.heroku.com/) is described [in the README of the backend project](https://github.com/magpie-ea/magpie-backend#deployment-with-heroku).

## Installation locally

The first-time installation requires an internet connection. After it is finished, the server can be launched offline.

(Note that for local deployment, the default username is `default` and the default password is `password`. You can change them in `config/dev.exs`.)

### First time installation

To begin with, install Docker from [https://docs.docker.com/install/](https://docs.docker.com/install/). You may have to launch the application once in order to let it install its command line tools. Verify the installation by typing `docker version` in a terminal (e.g., the Terminal app on macOS or cmd.exe on Windows).

Note:
- Although the Docker app on Windows and Mac asks for login credentials to Docker Hub, they are not needed for local deployment. You can proceed without creating any Docker account/logging in.
- Linux users would need to install `docker-compose` separately. See relevant instructions [here](https://docs.docker.com/compose/install/).

Once you have Docker installed, follow these steps:

2. Ensure you have [Git](https://git-scm.com/downloads) installed. Clone the server repo with `git clone https://github.com/magpie-ea/magpie-backend.git` or `git clone git@github.com:magpie-ea/magpie-backend.git`.

3. Open a terminal (e.g., the Terminal app on macOS or cmd.exe on Windows), `cd` into the project directory just cloned via git.

4. For the first-time setup, run in the terminal
  ```
  docker volume create --name magpie-app-volume -d local
  docker volume create --name magpie-db-volume -d local
  docker-compose run --rm web bash -c "mix deps.get && npm install && node node_modules/brunch/bin/brunch build && mix ecto.migrate"
  ```

### Deployment

After first-time installation, you can launch a local server instance which allows you to manage the experiments in your browser and stores the results.

1. Run `docker-compose up` to launch the application every time you want to run the server. Wait until the line `web_1  | [info] Running magpie-backend.Endpoint with Cowboy using http://0.0.0.0:4000` appears in the terminal.

2. Visit `localhost:4000` in your browser. You should see the server up and running.

    Note: Windows 7 users who installed *Docker Machine* might need to find out the IP address used by `docker-machine` instead of `localhost`. See [Docker documentation](https://docs.docker.com/get-started/part2/#build-the-app) for details.

3. Use <kbd>Ctrl + C</kbd> to shut down the server.

Note that the database for storing experiment results is stored at `/var/lib/docker/volumes/magpie-db-volume/_data` folder by default. As long as this folder is preserved, experiment results should persist as well.
