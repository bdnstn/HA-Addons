# Changelog
This project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.4.0] - 2024-08-30
### Security
- Mapp required ports
- Remove Host network access
- Add Apparmor
- Security rating is now 6
### Changed
- CGateWeb KeepAlive interval increased to 10 seconds to make logs more readable
- More detail in DOCS.

## [0.3.5] - 2024-08-29
### Changed
- CGateweb logging option changed to integer to allow log detail level selection in the future.

## [0.3.4] - 2024-08-25
### Added
- codenotary
- Added a delay to start of cgateweb to allow C-Gate to get up and running. Solves a couple of errors.
### Changed
- Default message interval change to 200 ms. Seems to fix CNI busy error on startup.
- Minor enhancement to log messages.
- Improved some of the parameter descriptions.

## [0.3.3] - 2024-08-25
### Added
- Changelog
- Documentation

## [0.3.4] - 2024-08-25
### Added
- codenotary
- Added a delay to start of cgateweb to allow C-Gate to get up and running. Solves a couple of errors.
### Changed
- Default message interval change to 200 ms. Seems to fix CNI busy error on startup.
- Minor enhancement to log messages.
- Improved some of the parameter descriptions.

