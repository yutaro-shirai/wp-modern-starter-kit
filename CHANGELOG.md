# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- **i-Willink DS primitives** via [`@willink-labs/css-tokens`](https://github.com/willink-oss/willink-design-system/tree/main/packages/css-tokens) `^1.2.0`:
  - `tokens.scale.css` (primitives-only) imported at the top of `src/css/style.css`, inlined into `dist/style.css` at build time
  - Primitives-only contract: radius / duration / easing / shadow / numeric color scales — DS semantic brand roles are NOT imported (brand colors stay theme-owned)
  - See README "i-Willink DS primitives" for override mechanics and the postcss-import `#exports` resolution note
- **23 new UI components** for comprehensive WordPress theming:
  - **Atoms (8)**: checkbox, radio, select, toggle, divider, progress, rating, tooltip
  - **Molecules (8)**: pagination, tabs, stat, steps, dropdown, accordion, menu, form-group
  - **Organisms (7)**: navbar, footer, modal, timeline, contact-form, gallery, team
- Devcontainer configuration (`.devcontainer/`) for VS Code development
- Enhanced CI/CD pipeline with PHP syntax checking
- Comprehensive component documentation in `docs/component-usage.md`

### Changed
- Updated GitHub Actions to Node.js 20 and actions v4
- Improved component library page (`page-components.php`) with all new components

### Fixed
- N/A

## [1.0.0] - 2024-01-01

### Added
- Initial release
- Base WordPress theme structure
- Tailwind CSS integration
- Atomic design component system (atoms, molecules, organisms)
- wp-env development environment
- GitHub Actions CI/CD workflow

---

[Unreleased]: https://github.com/i-Willink-Inc/wp-modern-starter-kit/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/i-Willink-Inc/wp-modern-starter-kit/releases/tag/v1.0.0
