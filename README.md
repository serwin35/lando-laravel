# 🚀 Lando Laravel Starter

[![Laravel](https://img.shields.io/badge/Laravel-FF2D20?style=flat-square&logo=laravel&logoColor=white)](https://laravel.com)
[![PHP](https://img.shields.io/badge/PHP_8.4-777BB4?style=flat-square&logo=php&logoColor=white)](https://www.php.net)
[![Lando](https://img.shields.io/badge/Lando-F06292?style=flat-square&logo=lando&logoColor=white)](https://lando.dev)
[![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)

A ready-to-use Lando configuration for Laravel development. Clone, start, and code — no Docker headaches.

## 📦 Stack

| Service | Version |
|---|---|
| PHP | 8.4 |
| MySQL | 8.4 |
| Node.js | 22 LTS |
| Redis | latest |
| Nginx | latest |
| MailHog | latest |
| phpMyAdmin | latest |
| Horizon | included |

## ⚡ Quick Start

### Prerequisites
- [Install Lando](https://docs.lando.dev/)
- [Trust the Lando CA](https://docs.lando.dev/config/security.html#trusting-the-ca)

### Installation
```bash
# Start containers
lando start

# Seed database
lando artisan db:seed

# Create storage symlink
lando artisan storage:link

# Create admin user
lando artisan user:create

# Start frontend watcher
lando watch
```

> 💡 After changing `.lando.yml`, run `lando rebuild -y`

## 🔗 Local URLs

| Service | URL |
|---|---|
| App | https://laravel-app.lndo.site |
| MailHog | http://mail.lndo.site |
| phpMyAdmin | http://phpmyadmin.lndo.site |

## ⌨️ Useful Commands

```bash
lando start             # Start all services
lando stop              # Stop all services
lando rebuild -y        # Rebuild after config changes
lando mysql             # MySQL CLI
lando ssh -s appserver  # SSH into PHP container
lando logs -s appserver # View PHP logs
```

### Development Tools
```bash
lando composer install  # Run Composer
lando npm install       # Run npm
lando watch             # Start Vite dev server
lando artisan migrate   # Run migrations
```

### Code Quality
```bash
lando phpunit           # Run PHPUnit tests
lando phpstan           # Static analysis
lando pint              # Laravel Pint (code style)
lando php-cs-fixer fix  # PHP CS Fixer
lando phpcs             # PHP CodeSniffer
lando coverage-report   # Generate coverage report
```

### Horizon
```bash
lando horizon:terminate # Restart Horizon worker
```

## ⚙️ GitHub Actions

CI/CD is pre-configured. Add these secrets to your repository:

| Secret | Description |
|---|---|
| `MAIN_SSH_KEY` | SSH deploy key for production |
| `MAIN_LARAVEL_ENV` | Production .env contents |
| `DEVELOP_SSH_KEY` | SSH deploy key for staging |
| `DEVELOP_LARAVEL_ENV` | Staging .env contents |

### Deployment branches
- `main` → production
- `develop` → staging

## 🌿 Branch Strategy

```
main            # Production
develop         # Staging
feature/*       # New features
releases/*      # Release candidates
```

## 📝 License

This project is open-sourced under the [MIT License](LICENSE).
