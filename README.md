# Ruby on Rails Devcontainer
Ruby on Rails devcontainer setup that includes Ruby, Ruby on Rails, PostgreSQL, Redis.

## Pre-requisits

- [VSCode](https://code.visualstudio.com)
- [Docker Desktop](https://www.docker.com/products/docker-desktop)

## What are we setting up in this devcontainer?

Softwares:
- Latest version of [Git](https://git-scm.com) version control system
- Version `3.2.4` of [Ruby](https://www.ruby-lang.org) programming language
- Version `20` of [NodeJS](https://nodejs.org) JavaScript runtime environment
- [PostgreSQL](https://www.postgresql.org) database based on the latest official docker image
- [Redis](https://redis.io) in-memory key-value store based on the latest official docker image
- [`postgresql-client`](https://www.postgresql.org/docs/current/app-psql.html)

Extensions:
- [Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)
- [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)
- [Ruby Solargraph](https://marketplace.visualstudio.com/items?itemName=castwide.solargraph)
- [ruby-rubocop-revived](https://marketplace.visualstudio.com/items?itemName=LoranKloeze.ruby-rubocop-revived)


## How to use?

Copy the `.devcontainer` directory into your Ruby on Rails project root directory and open it inside VSCode, then you will be able to use devcontainers as documented [here](https://code.visualstudio.com/docs/devcontainers/tutorial).

## Things to be aware of

### Automatic setup

This devcontainer setup does the following for you while building the image:
- Runs `gem update --system`
- Runs `bundle install`
- Runs `rails db:prepare`

You can customize this behavior inside `.devcontainer/setup.sh` file.

### PostgreSQL

You need to set the `host` and `port` properties for your development and testing databases in `config/database.yml` like this:

```yml
development:
  <<: *default
  database: ror_project_development

  username: postgres
  password: postgres
  host: db
  port: 5432

test:
  <<: *default
  database: ror_project_test

  username: postgres
  password: postgres
  host: db
  port: 5432
```

This is required because the docker service for PostgreSQL is named as `db` inside `.devcontainer/docket-compose.yml`, and you should access it by its name.

If you changed the `username` and/or `password` inside `config/database.yml`, then you will need to change them inside `.devcontainer/devcontainer.json` file to get `sqltools` extensions works properly.

### Redis

To connect to Redis in the development environment, you need to instantiate the connection like this:

```ruby
redis = Redis.new(url: ENV['REDIS_URL'] || 'http://redis:6379')
```

You should define `REDIS_URL` in your production environment, while in development `redis://redis:6379` will be used. This is required because the docker service for Redis is named as `redis` inside `.devcontainer/docket-compose.yml`, and you should access it by its name.

## Notes

- This is just a development setup, make sure to setup these services in your production environment also if you are using them
- Rebuild your devcontainer image when you change any file inside `.devcontainer` directory
- Take a look at the installed extensions and modify the list as required
- This could be used in GitHub Codespaces easily with a single click
- Feel free to submit any change you feel it is benefitial to this repo :)
