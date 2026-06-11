# edulution Plugins Repository

This repository contains a collection of **Docker Compose files** and **Traefik configurations** that cover various use cases. It is used for the installation of edulution extensions and can also be used to roll out applications independently.

## Contents of the repository

### 1. **Docker Compose files**

**Setup:** A customized Docker-Compose to be installed within edulution.

### 2. **Traefik configurations**

**Dynamic configurations:** YAML files that define dynamic routes, services and middlewares used for redirection within edulution.

## Repository structure

Plugins are grouped by edulution app under `apps/<app>/<container>/`. Each container directory holds the files that edulution-ui fetches at install time:

```
apps/
└── <app>/                       # e.g. filesharing
    └── edulution-<container>/    # e.g. edulution-eurooffice
        ├── docker-compose.yml    # required – the service definition
        └── filesharing.yml       # optional – Traefik dynamic configuration
```

edulution-ui loads `docker-compose.yml` directly from this repository (raw GitHub) when an application is installed.

## Available applications

| edulution app | Container directory | Purpose | Main image | Traefik route |
| --- | --- | --- | --- | --- |
| classmanagement | `edulution-veyon` | Classroom/PC management via Veyon WebAPI | `veyon/webapi-proxy` | – |
| desktopdeployment | `edulution-guacamole` | Clientless remote desktop gateway (Apache Guacamole) | `guacamole/guacamole`, `guacamole/guacd` | `/guacamole` |
| filesharing | `edulution-onlyoffice` | ONLYOFFICE document editor | `onlyoffice/documentserver` | `/docservice/` |
| filesharing | `edulution-collabora` | Collabora Online document editor | `collabora/code` | `/collabora` |
| filesharing | `edulution-eurooffice` | Euro-Office document editor (FOSS ONLYOFFICE fork) | `ghcr.io/euro-office/documentserver` | `/eurooffice/` |
| learningmanagement | `edulution-moodle` | Moodle learning management system | `ghcr.io/edulution-io/edulution-moodle` | `/moodle-app` |
| mail | `edulution-mail` | Mail stack (SOGo webmail) | `ghcr.io/edulution-io/edulution-mail` | `/sogo-mail` |
| wireguard | `edulution-wireguard` | WireGuard VPN gateway | `ghcr.io/edulution-io/edulution-wireguard` | – |
| edulution-manager | `edulution-manager-agent` | Management agent for edulution | `ghcr.io/edulution-io/edulution-manager/edulution-manager-agent` | – |

> The three `filesharing` editors are mutually exclusive alternatives – pick one as the active document editor.

## Configuration placeholders

Values inside the compose files use two different placeholder syntaxes that edulution-ui treats differently:

| Syntax | Example | Handling in edulution-ui |
| --- | --- | --- |
| `<...>` | `<EDULUTION_MAIL_HOSTNAME>` | Rendered as an **input field** in the installation dialog. The admin enters the value, which is then substituted into the compose file. |
| `${...}` | `${EDULUTION_EUROOFFICE_JWT_SECRET}` | Resolved from the **API environment** (`appConfigValues` / `process.env`). Secrets (passwords, JWT secrets, …) that have no value are **auto-generated** and persisted. Supports `${VAR:-default}` fallbacks. |

Rule of thumb: use `<...>` for values the admin must provide (hostnames, external URLs), and `${...}` for values that come from the edulution environment or should be generated automatically (secrets, passwords, tokens).

## Prerequisites

- **Docker:** Version 27.5.0 or newer
- **Docker Compose:** Version 2.32.0 or newer
- **Traefik:** Version v3.1 or newer
