---
order: 95
---
# Easypanel

[Easypanel](https://easypanel.io/) is a modern server control panel that can deploy 2FAuth with one click using its official template, without needing to manually run Docker commands.

---

## Installation

1. Open your Easypanel dashboard and create (or open) a project
2. Click __+ Add Service__ and choose __Templates__
3. Search for __2FAuth__ and select it
4. Optionally adjust the database type (SQLite, PostgreSQL, MariaDB or MySQL) and enable Redis
5. Click __Create__ to deploy the service

:::desktop-screen
![2FAuth deployed on Easypanel](/static/easypanel_deployed.png)
:::

Easypanel runs the [official 2FAuth Docker image](https://hub.docker.com/r/2fauth/2fauth) for you and exposes it through a domain, so no manual web server configuration is required.

See the [official 2FAuth template on Easypanel](https://easypanel.io/templates/2fauth) for more details.
