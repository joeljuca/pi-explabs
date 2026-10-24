# Changelog

Based on [Keep a Changelog](https://keepachangelog.com).

<!--
Templates:

    ## [Unreleased]

    ### Added

    ### Changed

    ### Fixed

    ### Removed

    ## [X.Y.Z] - 1970-01-01

    ### Added

    ### Changed

    ### Fixed

    ### Removed
-->

## [Unreleased]

### Added

- Read the API key from pi's `auth.json` (`explabs` entry), supporting literals, `$ENV_VAR` references and `!command` secret lookups, with `EXPLABS_API_KEY` as a fallback

### Changed

- `/login explabs` now points at `auth.json` and `EXPLABS_API_KEY`
- The `/models` fetch now uses the credential pi resolved, runs once per refresh (it used to run twice), and is skipped when pi refreshes offline or no API key is configured

### Fixed

### Removed

## [0.1.0] - 2026-09-09

### Added

- First release

[Unreleased]: https://github.com/joeljuca/pi-explabs/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/joeljuca/pi-explabs/releases/tag/v0.1.0
