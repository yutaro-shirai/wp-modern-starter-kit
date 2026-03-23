[日本語 (Japanese)](README.ja.md)

# Modern WordPress Starter Kit

[![CI](https://github.com/i-Willink-LLC/wp-modern-starter-kit/actions/workflows/main.yml/badge.svg)](https://github.com/i-Willink-LLC/wp-modern-starter-kit/actions/workflows/main.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)
[![Node.js](https://img.shields.io/badge/Node.js-%3E%3D18.17.0-green.svg)](https://nodejs.org/)

A starter kit that dramatically accelerates PHP-based WordPress theme development.
Featuring modern styling with Tailwind CSS, pre-built UI components, and automated build/deploy workflows via GitHub Actions, it slashes lead time from kickoff to delivery in client projects.

---

## Why This Kit?

- **Atomic Design Components** -- A library of production-ready UI parts (Hero, Cards, CTA, Breadcrumbs, and more) organized by Atoms / Molecules / Organisms, so you never start from scratch.
- **Zero-Config Local Dev** -- `wp-env` gives you a full WordPress environment with a single command. No manual Docker setup, no MAMP/XAMPP headaches.
- **Auto-Release ZIP** -- Push a Git tag and GitHub Actions builds, packages, and publishes a deployment-ready ZIP to GitHub Releases.
- **Pure PHP, No Framework Lock-in** -- Works with standard `get_template_part()`. No React, no Vue, no build-time SSR -- just PHP templates that any WordPress developer can maintain.

---

## What's Included

- Tailwind CSS with utility-first styling and componentized template parts
- Pre-built UI components: Hero, Cards, CTA, Pricing, Breadcrumbs, and more
- GitHub Actions CI/CD pipeline (lint, build, auto-release)
- PHPCS (WordPress Coding Standards) + Prettier for code quality
- Devcontainer support for a consistent development environment
- Hot reload during development

---

## Table of Contents

- [Features](#features)
- [Project Structure](#project-structure)
- [Quick Start](#quick-start)
- [Development Guide](#development-guide)
- [Build and Delivery](#build-and-delivery)
- [Available Commands](#available-commands)
- [Documentation](#documentation)
- [Contributing](#contributing)
- [License](#license)

---

## Features

| Technology | Description |
|---|---|
| **Tailwind CSS** | Utility-first styling with componentized `template-parts` |
| **Pre-built UI** | Production-ready components: Hero, Cards, CTA, and more |
| **Pure PHP** | No frontend framework dependency -- works with PHP alone |
| **Auto Release** | GitHub Actions automatically generates delivery-ready ZIP files |
| **Modern DX** | Zero-config local development with `wp-env` and hot reload |
| **Code Quality** | PHPCS (WordPress Coding Standards) + Prettier |
| **Devcontainer** | Unified development environment -- eliminates setup friction |

---

## Project Structure

```
wp-modern-starter-kit/
├── src/                     # Source code (build target)
│   ├── css/                 # Tailwind CSS / SCSS
│   └── js/                  # TypeScript / JavaScript
├── parts/                   # Reusable template parts
│   ├── atoms/               # Atomic Design: smallest components
│   ├── molecules/           # Atomic Design: composite components
│   ├── organisms/           # Atomic Design: complex components
│   ├── common/              # Header, Footer, etc.
│   └── layouts/             # Layout definitions
├── inc/                     # PHP logic (split from functions.php)
├── templates/               # Individual page templates
├── docs/                    # Documentation
│   └── architecture.md      # Architecture design document
├── functions.php            # Theme entry point
├── style.css                # Theme definition file
└── .github/workflows/       # CI/CD (Lint, Release)
```

---

## Quick Start

### Prerequisites

| Tool | Minimum Version | Recommended |
|------|----------------|-------------|
| **Node.js** | 18.17.0 | 20.x LTS |
| **Docker** | -- | Latest (required for wp-env / Devcontainer) |

### 1. Clone the Repository

```bash
git clone https://github.com/i-Willink-LLC/wp-modern-starter-kit.git my-theme
cd my-theme
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Start the Local Environment (wp-env)

Spins up a local WordPress environment using Docker.
**Docker Desktop** or **OrbStack** must be installed and running.

```bash
npm run env:start
```

> [!NOTE]
> The first launch may take a few minutes as it downloads WordPress core and database images.

*   **Site URL**: http://localhost:8888
*   **Admin Panel**: http://localhost:8888/wp-admin
    *   User: `admin`
    *   Password: `password`

### 4. Start the Development Server

Watches for file changes and triggers automatic builds with hot reload.

```bash
npm run dev
```

### Using Devcontainer (Recommended)

1. Start Docker Desktop or Rancher Desktop
2. Open the project in VS Code
3. Command Palette (Ctrl+Shift+P) -> **"Dev Containers: Reopen in Container"**

> [!TIP]
> Using a Devcontainer means Node.js and all required tools are set up inside the container, keeping your local machine clean.

---

## Development Guide

### Using Components

This kit includes UI components under `parts/`, organized following Atomic Design principles.
Use PHP's `get_template_part` function to call them with arguments.

**Example: Displaying a Hero Section**

```php
<?php
get_template_part('parts/organisms/hero-simple', null, [
    'title'       => 'Your Company Name',
    'description' => 'Accelerate your business with cutting-edge technology.',
    'button_text' => 'Contact Us',
    'button_url'  => '/contact',
    'image_url'   => 'https://example.com/hero.jpg'
]);
?>
```

**Example: Displaying a Button (Atom)**

```php
<?php
get_template_part('parts/atoms/button', null, [
    'text'    => 'Learn More',
    'variant' => 'primary',
    'size'    => 'lg'
]);
?>
```

**Example: Displaying Breadcrumbs (Molecule)**

```php
<?php
get_template_part('parts/molecules/breadcrumbs', null, [
    'items' => [
        ['label' => 'Home', 'url' => '/'],
        ['label' => 'Services', 'url' => '/services'],
        ['label' => 'Details']  // Last item has no link
    ]
]);
?>
```

### Styling

Edit `src/css/style.css`. Use Tailwind CSS utility classes directly, or define custom classes using the `@apply` directive as needed.

### Viewing the Component Library

After starting the local environment, you can preview all components and their usage on the component library page.

*   **Component Library**: http://localhost:8888/?page_id=5

---

## Build and Delivery

### Production Build

Optimizes (minifies) CSS/JS for production.

```bash
npm run build
```

### Packaging the Theme (Manual)

Excludes unnecessary files (node_modules, src, etc.) and creates a ZIP archive of the theme.

```bash
npm run zip
```

This generates `dist/wp-modern-starter-kit.zip`.

### Auto Release (GitHub Actions)

Push a Git tag to automatically trigger a build, create a ZIP, and upload it to GitHub Releases.

```bash
git tag v1.0.0
git push origin v1.0.0
```

---

## Available Commands

| Command | Description |
|---------|-------------|
| `npm run dev` | Start development mode (Watch + Hot Reload) |
| `npm run build` | Production build |
| `npm run env:start` | Start local WordPress |
| `npm run env:stop` | Stop local WordPress |
| `npm run lint` | Code quality check (PHP/JS/CSS) |
| `npm run format` | Auto-format code |
| `npm run zip` | Generate delivery ZIP |

---

## Documentation

For detailed documentation, see the `docs/` directory.

| Document | Description |
|----------|-------------|
| [architecture.md](docs/architecture.md) | Architecture design based on Atomic Design |

---

## Contributing

Contributions are welcome! See [CONTRIBUTING.md](CONTRIBUTING.md) for details.

---

## License

This project is licensed under the [MIT License](https://opensource.org/licenses/MIT).
