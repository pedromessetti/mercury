# Requirements - MercuryV2 CI Pipeline

This Markdown file is used to store Mercury HF Modem V2 requirements for the current CI pipeline.

Each requirement shall contain:
- Requirement ID, composed as: **R-CI-<NUMERICAL_ID>**
- Requirement title
- Description of the requirement
- User Acceptance Test (UAT) - Concrete acceptance criterion that proves the requirement is met. CI must execute that step to verify the requirement.
- Status: Not Started / Started / In Review / Done / Postponed / Obsolete
- Version - Shall be updated on each change of the current requirement


## Requirements

<!-- Core CI pipeline - everything needed for automated quality gates on PRs -->

### R-CI-001: Linux Build Verification (amd64)
Compile Mercury on debian13 (amd64) with `make` - must exit 0 with no errors

**UAT:** `make clean && make` succeeds on debian13 runner

**Status:** Done

**Version:** 1.0

### R-CI-002: Linux Build Verification (arm64)

Cross-compile Mercury for arm64 using `gcc-aarch64-linux-gnu`

**UAT:** Cross-compilation succeeds with `CC=aarch64-linux-gnu-gcc make`

**Status:** Done

**Version:** 1.0

### R-CI-003: Windows Cross-Compile Verification

Cross-compile Mercury for Windows using MinGW-w64 (`make windows`)

**UAT:** `make windows` succeeds on debian13 with MinGW installed

**Status:** Done

**Version:** 1.0

### R-CI-004: Static Analysis with cppcheck

Run cppcheck on Mercury source code, excluding vendor directories (`modem/freedv/`, `gui_interface/websocket/mongoose.c`, `audioio/ffaudio/`)

**UAT:** cppcheck runs with `--error-exitcode=1` and reports zero errors on clean code; vendor directories are excluded

**Status:** Postponed

**Reason:** SCA would be very helpful to catch, fix and prevent bugs in the future but current CI is in early state and it's focused on stages that will help on fast development and automate verification tasks

**Version:** 0.1

### R-CI-005: Selective Warning Enforcement

Enforce `-Werror` on Mercury's own code while allowing warnings from vendor code

**UAT:** Compiler warnings in Mercury's own files cause build failure; vendor file warnings are suppressed

**Status:** Postponed

**Reason:** SCA will be very helpful to catch, fix and prevent bugs in the future but current CI is in early state and it's focused on building stages that will help on fast development and automate verification tasks

**Version:** 0.1

### R-CI-006: Unit Test Framework Setup (Unity + FFF)

Integrate Unity test framework and FFF mocking into `tests/` directory with `make test` target

**UAT:** `make test` builds and runs test suite, exits 0 when all tests pass, non-zero on failure

**Status:** Done

**Version:** 0.1

### R-CI-007: ARQ Protocol Unit Tests

Unit tests for ARQ protocol logic - CALL/ACCEPT handshake, ACK/NAK, state transitions, gear-shifting, timeout handling

**UAT:** Tests cover core FSM transitions and protocol correctness; all pass

**Status:** Not started

**Version:** 0.1

### R-CI-008: TCP TNC Interface Unit Tests

Unit tests for VARA-compatible TCP TNC command parsing and status emission

**UAT:** Tests verify parsing of MYCALL, LISTEN, CONNECT, DISCONNECT, BW, RETRY commands and correct status responses

**Status:** Not started

**Version:** 0.1

### R-CI-009: WebSocket Command Tests

Tests for WebSocket command send/receive between GUI and mercury backend

**UAT:** Tests verify command dispatch and status JSON generation for key UI interactions

**Status:** Not started

**Version:** 0.1

### R-CI-010: Broadcast Test Utils Integration

Integrate existing broadcast test utilities from `utils/` into CI pipeline

**UAT:** Broadcast test tools build and run successfully in CI; results reported

**Status:** Not started

**Version:** 0.1

### R-CI-011: Debian Package Build (amd64)

Build `.deb` package for amd64 using existing `debian/` directory

**UAT:** `.deb` package builds successfully and is uploaded as CI artifact

**Status:** Not started

**Version:** 0.1

### R-CI-012: Debian Package Build (arm64)

Cross-build `.deb` package for arm64

**UAT:** `.deb` package for arm64 builds successfully and is uploaded as CI artifact

**Status:** Not started

**Version:** 0.1

### R-CI-013: Dummy-Load Hardware Tests

Run end-to-end transmission tests on LAB stations (dummy loads) via SSH - 5 test payloads (0B, <200B, 512B, 2.5-5KB, image ≤10KB)

**UAT:** All 5 payloads transmit and verify (sha256 match) on dummy-load station pair; test report generated

**Status:** Not started

**Version:** 0.1

### R-CI-014: OTA Hardware Tests

Run end-to-end transmission tests on AIR1 stations (500km OTA) via SSH - same 5 test payloads

**UAT:** All 5 payloads transmit and verify on OTA station pair; test report generated (informational - not blocking)

**Status:** Not started

**Version:** 0.1

### R-CI-015: Station Concurrency Control

Prevent simultaneous hardware tests from conflicting on shared physical stations

**UAT:** Concurrent PR triggers queue rather than conflict; only one hardware test runs per station pair at a time

**Status:** Not started

**Version:** 0.1

## Requirements for Phase 2 (P2)

<!-- Future enhancements after v1 is stable -->

- [ ] Code coverage reporting
- [ ] clang-tidy integration
- [ ] Performance benchmarking
- [ ] CD/deployment pipeline
- [ ] mercury-qt GUI CI (separate repo)
- [ ] Automated release creation with .deb artifacts

## Out of Scope

- FreeDV/codec2 vendored library testing - upstream responsibility
- Mongoose WebSocket library testing - vendor testing not needed
- macOS and Android platform CI - currently marked UNSUPPORTED in code
- Self-hosted GitHub Actions runners - using SSH instead

## Traceability

| Req ID | PROJECT.md Active Item | Phase |
|--------|----------------------|-------|
| R-CI-001 | Build verification (Linux amd64) | Phase 1 |
| R-CI-002 | Build verification (arm64) | Phase 1 |
| R-CI-003 | Build verification (Windows cross) | Phase 1 |
| R-CI-004 | Static analysis (cppcheck) | Phase 2 |
| R-CI-005 | Selective -Werror | Phase 2 |
| R-CI-006 | Unit test framework | Phase 3 |
| R-CI-007 | ARQ protocol unit tests | Phase 4 |
| R-CI-008 | TCP TNC interface tests | Phase 5 |
| R-CI-009 | WebSocket command tests | Phase 6 |
| R-CI-010 | Broadcast test utils | Phase 7 |
| R-CI-011 | Debian .deb (amd64) | Phase 8 |
| R-CI-012 | Debian .deb (arm64) | Phase 8 |
| R-CI-013 | Dummy-load hardware tests | Phase 10 |
| R-CI-014 | OTA hardware tests | Phase 11 |
| R-CI-015 | Station concurrency | Phase 9-10 |
