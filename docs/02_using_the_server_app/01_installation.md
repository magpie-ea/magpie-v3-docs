(Please refer to the [magpie-backend Github repo](https://github.com/magpie-ea/magpie-backend) for the most up-to-date detailed instructions.)

## Deploying the Server

This section documents some methods one can use to deploy the server.

Previously, Heroku was the recommended way to deploy the server. However, it ceased support for its free plan. Gigalixir and Fly.io are listed below as potential alternatives.

### Deploying via Gigalixir

[Gigalixir](https://www.gigalixir.com) is a hosting service that offers a free tier. However, note that deploying the app once per month might be needed to avoid the app being temporarily shut down.

```
pip3 install gigalixir --user
gigalixir signup
gigalixir login
# Replace [your-app-name] with the desired name.
gigalixir create -n [your-app-name]
gigalixir pg:create --free
# Replace [your-app-name] with the desired name.
gigalixir config:set PHX_HOST=[your-app-name].gigalixirapp.com
gigalixir config:set PHX_SERVER=true
# Replace with your desired auth username and password.
gigalixir config:set AUTH_USERNAME=[your-auth-username]
gigalixir config:set AUTH_PASSWORD=[your-password]
git push -u gigalixir master
```

```
HTTPoison.get(url, [], [proxy: {:socks5, String.to_charlist("server_domain"), port_num}, socks5_user: "username", socks5_pass: "password"])
```

To deploy the app again after pulling in the latest updates:

```
# The `origin` git remote should point to the Github repo, while the `gigalixir` git remote points to the Fly.io repo.
git pull origin master
git push -u gigalixir master
```

To force deploy the app (e.g. once per month) even without any changes:

```
git commit --amend --no-edit
git push --force -u gigalixir master
```

### Deploying via Fly.io

[Fly.io](https://fly.io/) is a hosting service that remains free as long as the app's monthly usage remains below [the allowance](https://fly.io/docs/about/pricing/#free-allowances). However, credit card information is needed upon signup.


```
brew install flyctl
fly auth signup
fly auth login
# Do not proceed to deployment yet at the "whether to deploy" step.
fly launch
# Replace with your desired auth username and password.
fly secrets set AUTH_USERNAME=[your-auth-username]
fly secrets set AUTH_PASSWORD=[your-password]
fly deploy
```

To deploy the app again after pulling in the latest updates:

```
# The `origin` git remote should point to the Github repo, while the `gigalixir` git remote points to the Fly.io repo.
git pull origin master
fly deploy
```

### Running the app locally

To run the server app locally with `dev` environment, the following instructions could help. However, as the configuration of Postgres DB could be platform specific, relevant resources for [Postgres](https://www.postgresql.org/) could help.

1. Install Postgres. Ensure that you have version 9.2 or greater (for its JSON data type). You can check the version with the command `psql --version`.

2. Make sure that Postgres is correctly initialized as a service. If you installed it via Homebrew, the instructions should be shown on the command line. If you're on Linux, [the guide on Arch Linux Wiki](https://wiki.archlinux.org/index.php/PostgreSQL#Initial_configuration) could help.

3. Run `mix deps.get; mix ecto.create; mix ecto.migrate` in the app folder.

4. Run `mix assets.deploy` to deploy the frontend assets using `esbuild`.

5. Run `cd ..; mix phx.server` to run the server on `localhost:4000`.

6. Every time a database change is introduced with new migration files, run `mix ecto.migrate` again before starting the server.

