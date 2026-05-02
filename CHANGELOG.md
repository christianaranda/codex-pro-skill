# Changelog

All notable changes to this project are documented in this file.

This project follows [Semantic Versioning](https://semver.org/spec/v2.0.0.html), and this changelog follows the structure from [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

### Changed

- Clarify that Pro browser work must use `$browser-use:browser` with the in-app browser `iab` backend, not standalone Playwright or another browser automation path.

## [0.1.3] - 2026-05-01

### Added

- Add Pro thread selection guidance: use a user-specified Pro thread when provided, otherwise reuse the relevant existing Pro thread when it preserves useful context, and start a clean thread when the prior context is unrelated or stale.

## [0.1.2] - 2026-04-30

### Changed

- Switch the repository to skill-first distribution with the installable skill at `skills/pro`.
- Simplify the README around the canonical `$skill-installer` install path.

### Removed

- Remove the experimental plugin marketplace wrapper from the public package.

## [0.1.1] - 2026-04-30

### Changed

- Rework the earlier plugin marketplace layout to match Codex marketplace conventions.

## [0.1.0] - 2026-04-30

### Added

- Initial public packaging for the Pro Review Orchestration skill.
