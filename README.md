# strapi (v4+) containerized

![Strapi](https://github.com/V-Shadbolt/docker-strapi/blob/main/assets/PNG.logo.purple.dark.png?raw=true)

> Docker image for strapi version 4 (latest version)

API creation made simple, secure and fast. The most advanced open-source Content Management Framework to build powerful
API with no effort.

[GitHub repository](https://github.com/V-Shadbolt/docker-strapi)

[Docker Hub](https://hub.docker.com/r/vshadbolt/strapi)

[![Docker Pulls](https://img.shields.io/docker/pulls/vshadbolt/strapi.svg?style=for-the-badge)](https://hub.docker.com/r/vshadbolt/strapi)

---

# Example

Using Docker Compose, create `docker-compose.yml` file with the following content:

```yaml
version: "3"
services:
  strapi:
    container_name: strapi
    image: vshadbolt/strapi
    ports:
      - "1337:1337"
    environment:
      NODE_ENV: development # or production
    # volumes:
    #   - ./app:/srv/app # mount an existing strapi project
```

or using Docker:

```shell
docker run -d -p 1337:1337 vshadbolt/strapi --env NODE_ENV=development
```

You can find more examples on [GitHub](https://github.com/vshadbolt/docker-strapi/tree/main/examples).

---

# How to use ?

This image allows you to create a new strapi project or run an existing strapi project.

- for `$NODE_ENV = development`: The command that will run in your project
  is [`strapi develop`](https://docs.strapi.io/developer-docs/latest/developer-resources/cli/CLI.html#strapi-develop).
- for `$NODE_ENV = production`: The command that will run in your project
  is [`strapi start`](https://docs.strapi.io/developer-docs/latest/developer-resources/cli/CLI.html#strapi-start).

> The [Content-Type Builder](https://strapi.io/features/content-types-builder) plugin is disabled WHEN `$NODE_ENV = production`.

Note that for existing projects, `config/admin.js`, `config/server.js`, and `config/middlewares.js` files will need to be configured correctly not to use `http://localhost:1337`.

---

## Creating a new strapi project

When running this image, strapi will check if there is a project in the `/srv/app` folder of the container. If there is
nothing then it will run
the [`strapi new`](https://docs.strapi.io/developer-docs/latest/developer-resources/cli/CLI.html#strapi-new)
command in the container /srv/app folder.

This command creates a project with an SQLite database. Then starts it on port `1337`.

This docker image will also update the `config/admin.js`, `config/server.js`, and `config/middlewares.js` files to accomodate setting image sources, CORS params, admin url(s), and public server url(s) from the docker command. See below for example configurations.

- Add `ADMIN_URL: tobemodified` to your docker command to set a custom domain for your admin page(s). Ex. `https://api.example.com/admin`. Without adding the command, config will default to `http://localhost:1337/admin`
- Add `PUBLIC_URL: tobemodified` to your docker command to set a custom domain for your api endpoint(s). Ex. `https://api.example.com` Without adding the command, config will default to `http://localhost:1337`
- Add `IMG_ORIGIN: "toBeModified1,toBeModified2"` to your docker command to allow new image sources for your project. Ex. `'self',data:,blob:,api.example.com,market-assets.strapi.io`. Without adding the command, config will default to `'self',data:,blob:`
- Add `CORS_ORIGIN: "toBeModified1,toBeModified2"` to your docker command to allow new CORS origin sources to your project. Ex. `https://myfrontendwebsite.example.com,https://api.example.com`. Without adding the command, config will default to `*`

- The official documentation for strapi and these files is linked below.

---

**Environment variables**

When creating a new project with this image you can pass in database configurations to
the [`strapi new`](https://strapi.io/documentation/developer-docs/latest/developer-resources/cli/CLI.html#strapi-new)
command. You're also able to add these configurations to your docker command for existing strapi projects

- `DATABASE_CLIENT` a database provider supported by Strapi: (sqlite, postgres, mysql ,mongo).
- `DATABASE_HOST` database host.
- `DATABASE_PORT` database port.
- `DATABASE_NAME` database name.
- `DATABASE_USERNAME` database username.
- `DATABASE_PASSWORD` database password.
- `DATABASE_SSL` boolean for SSL.
- `JWT_SECRET` random string
- `ADMIN_JWT_SECRET` random string
- `APP_KEYS` randomstring1,randomstring2
- `API_TOKEN_SALT` random string
- `TRANSFER_TOKEN_SALT` random string
- `EXTRA_ARGS` pass extra args to
  the [`strapi new`](https://strapi.io/documentation/developer-docs/latest/developer-resources/cli/CLI.html#strapi-new).

---

## Running an existing strapi project

To run an existing project, you can mount the project folder in the container at `/srv/app`.

---

## Modifying files in the /config directory

After modifying these files, the project will need to be rebuilt to account for the changes. To do so, add the following configuration to the docker command. Not that not adding this argument will negate any changes made within the `/config` directory until it is added. It is particularily important for any URL changes or your project will not be accessible.

- `BUILD: true`

---

## Updating packages on an existing strapi project

To update packages in an existing project, pass in the below command. Note that this will potentially upgrade packages across major versions and has the possibility of breaking the project. Please remember to backup responsibly.

- `UPGRADE: true`

---

# Recommended way to deploy an existing strapi project to production using Docker

To deploy an existing strapi project to production using Docker, it is recommended to build an image for your project
based on [node v18](https://hub.docker.com/_/node).

The official docker documentation for strapi, including example Dockerfiles, is available on [https://docs.strapi.io/dev-docs/installation/docker](https://docs.strapi.io/dev-docs/installation/docker).

---

# Official Documentation

- The official documentation of strapi is available on [https://docs.strapi.io/](https://docs.strapi.io/).

- The official strapi docker image is available on [GitHub](https://github.com/strapi/strapi-docker) (not yet upgraded
  to v4).
