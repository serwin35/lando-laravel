<div align="center">

# 🚀 Lando Laravel

[![Laravel](https://img.shields.io/badge/Laravel-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)](https://laravel.com)
[![PHP](https://img.shields.io/badge/PHP-8.4-777BB4?style=for-the-badge&logo=php&logoColor=white)](https://php.net)
[![Lando](https://img.shields.io/badge/Lando-EF5A8F?style=for-the-badge&logo=lando&logoColor=white)](https://lando.dev)
[![MySQL](https://img.shields.io/badge/MySQL-8.4-4479A1?style=for-the-badge&logo=mysql&logoColor=white)](https://mysql.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](LICENSE)

**Zero-config local Laravel development environment powered by Lando.**
Clone, run `lando start`, and you're coding in under a minute.

</div>

---

## What is Lando?

[Lando](https://lando.dev) is a local development environment tool built on top of Docker. It abstracts away the complexity of Docker Compose, giving you a dead-simple config file (`.lando.yml`) that spins up a fully isolated, production-like stack per project.

**Why Lando over raw Docker?**

| Pain point with raw Docker | How Lando solves it |
|---|---|
| Port conflicts between projects | Per-project isolation — no more `3306 already in use` |
| Manual HTTPS setup | Automatic trusted HTTPS via built-in proxy |
| Verbose `docker-compose.yml` files | Single `.lando.yml` — readable in seconds |
| Rebuilding containers from scratch | `lando rebuild` handles it cleanly |
| Running Artisan / Composer inside containers | `lando artisan`, `lando composer` — native CLI shortcuts |

---

## Stack

| Service | Version | Notes |
|---|---|---|
| **PHP** | 8.4 | With Xdebug enabled |
| **Nginx** | Latest | Serves the Laravel app |
| **MySQL** | 8.4 | Port-forwarded to `3307` |
| **Redis** | Latest | Cache, queues, sessions |
| **Node.js** | 22 LTS | Frontend asset compilation |
| **MailHog** | Latest | Catches all outgoing mail locally |
| **phpMyAdmin** | Latest | GUI for MySQL |
| **Laravel Horizon** | — | Queue monitoring dashboard |

### Proxy URLs

| Service | URL |
|---|---|
| Application | https://laravel-app.lndo.site |
| MailHog | http://mail.lndo.site |
| phpMyAdmin | http://phpmyadmin.lndo.site |

---

## Prerequisites

1. **Install Lando** — [https://docs.lando.dev/getting-started/installation.html](https://docs.lando.dev/getting-started/installation.html)
2. **Trust the Lando CA** (for valid HTTPS in your browser) — [https://docs.lando.dev/core/v3/security.html](https://docs.lando.dev/core/v3/security.html)
3. Docker Desktop (macOS/Windows) or Docker Engine (Linux) — installed automatically with most Lando packages.

---

## Quick Start

```bash
# 1. Clone the repository
git clone https://github.com/serwin35/lando-laravel.git
cd lando-laravel

# 2. Copy the environment file
cp .env.example .env

# 3. Start the environment
lando start

# 4. Install PHP dependencies
lando composer install

# 5. Install Node dependencies and build assets
lando npm install && lando npm run build

# 6. Generate the application key
lando artisan key:generate

# 7. Run database migrations (with optional seeder)
lando artisan migrate --seed
```

Open **https://laravel-app.lndo.site** — your app is live.

---

## Useful Commands

### Environment

```bash
lando start          # Start all services
lando stop           # Stop all services
lando rebuild        # Rebuild containers (after .lando.yml changes)
lando destroy        # Remove containers and volumes
lando info           # Show service URLs, ports, credentials
lando logs           # Tail logs from all services
lando logs -s appserver   # Logs for a specific service
lando ssh            # SSH into the appserver container
lando ssh -s database     # SSH into the database container
```

### Laravel / Artisan

```bash
lando artisan <command>          # Run any Artisan command
lando artisan migrate            # Run migrations
lando artisan migrate:fresh --seed  # Fresh migration + seed
lando artisan make:model Post -mcr  # Generate model, migration, controller
lando artisan tinker             # Open Tinker REPL
lando artisan route:list         # List all registered routes
lando artisan config:cache       # Cache configuration
```

### Composer & Node

```bash
lando composer require <package>   # Add a PHP package
lando composer update              # Update PHP dependencies
lando npm install                  # Install Node dependencies
lando npm run dev                  # Start Vite in watch mode
lando npm run build                # Production asset build
```

---

## Code Quality

All tools run inside the Lando container — no local PHP installation required.

```bash
# PHPUnit — run the full test suite
lando phpunit

# PHPUnit — with coverage report
lando phpunit --coverage-html coverage/

# PHPStan — static analysis (level configured in phpstan.neon)
lando phpstan analyse

# Laravel Pint — auto-fix code style
lando pint

# Laravel Pint — check without fixing (CI mode)
lando pint --test

# PHP CS Fixer — fix code style
lando php-cs-fixer fix

# PHP CodeSniffer — check coding standards
lando phpcs

# PHP CodeSniffer — auto-fix violations
lando phpcbf
```

---

## Laravel Horizon

Horizon provides a beautiful dashboard for monitoring your Redis queues.

```bash
# Start Horizon
lando artisan horizon

# Pause queue processing
lando artisan horizon:pause

# Resume queue processing
lando artisan horizon:continue

# Terminate Horizon (graceful restart)
lando artisan horizon:terminate

# Check Horizon status
lando artisan horizon:status
```

Access the dashboard at **https://laravel-app.lndo.site/horizon**.

> **Tip:** In production, protect `/horizon` with Horizon's built-in authorization gate in `HorizonServiceProvider`.

---

## GitHub Actions — CI/CD

This project ships with a GitHub Actions workflow for automated testing and code quality checks.

### Required Secrets

Configure these in **Settings → Secrets and variables → Actions**:

| Secret | Description |
|---|---|
| `APP_KEY` | Laravel application key (`php artisan key:generate --show`) |
| `DB_PASSWORD` | MySQL root password |
| `DB_DATABASE` | Database name used in CI |
| `REDIS_PASSWORD` | Redis password (if auth is enabled) |

### What the Pipeline Does

1. **Lint** — runs Laravel Pint in `--test` mode
2. **Static Analysis** — runs PHPStan
3. **Tests** — runs PHPUnit against a fresh MySQL + Redis matrix
4. **Build** — compiles frontend assets with Vite

---

## Branch Strategy

| Branch | Purpose |
|---|---|
| `main` | Stable, production-ready configuration |
| `feature/*` | New features or stack upgrades |
| `fix/*` | Bug fixes |

All changes to `main` require a passing CI build.

---

## Contributing

Contributions are welcome! If you spot an issue or have an idea for improving the stack:

1. Fork the repository
2. Create a branch: `git checkout -b feature/your-idea`
3. Commit your changes: `git commit -m "feat: describe your change"`
4. Push: `git push origin feature/your-idea`
5. Open a Pull Request

Please keep `.lando.yml` changes backward-compatible where possible and include a brief description of the problem your change solves.

---

## License

This project is open-source under the [MIT License](LICENSE).

---

<div align="center">

If this saved you setup time, consider giving it a ⭐ — it helps others find it.

Made with care for the Laravel community.

</div>
