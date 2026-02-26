# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a **pure Markdown documentation repository** — there is no build system, no code to compile, and no tests to run. The content documents SMC (Security Management Center) High Availability setup for Forcepoint NGFW, covering only SMC versions **7.3.0 and later** (which use native PostgreSQL replication instead of the legacy replication mechanism).

Browse locally using [Obsidian](https://obsidian.md/) opened on the repository root as a vault, or any Markdown-compatible viewer.

## Documentation Structure

```
README.md                          # Entry point and navigation index
docs/
  user-manual/                     # End-user documentation
    installation-information.md    # Prerequisites and info
    installation.md
    upgrade.md
    licensing.md
    backup-management.md
    gui-administration*.md         # SMC Client (GUI) administration
    gui-commands/                  # Individual GUI command docs
      advanced/                    # Advanced/risky GUI commands
    command-line-administration.md # Console commands overview
    console-commands/              # Individual console command docs
      advanced-commands/           # Advanced/risky console commands
    database-replication-states.md # Reference for all replication state codes
    troubleshooting.md             # Troubleshooting index
    troubleshooting/               # Individual troubleshooting scenarios
    screens/                       # Screenshots (PNG)
  api-automation/                  # API/automation documentation
    smc-api.md                     # SMC REST API overview and index
    smc-api-*.md                   # Individual SMC API entry point docs
    smc-python.md                  # SMC Python library overview
    smc-python-management-methods.md
    smc-python-example/            # Example Python scripts
    images/                        # Diagrams (PNG)
```

## Writing Conventions

- **Standard Markdown links only** — do not use Obsidian-specific wiki links (`[[...]]`). All links must be compatible with GitHub and any generic Markdown viewer.
- **Relative links** — links between docs use relative paths (e.g., `../api-automation/smc-api.md`).
- **Scope disclaimer** — any documentation added must apply only to SMC 7.3.0+ with native PostgreSQL replication. Clearly note version requirements (e.g., "SMC Python 1.0.33 or higher", "SMC 7.3.4, 7.4.2 or higher") when a feature or entry point has a minimum version requirement.
- **Risk labeling** — console commands and advanced operations must include a risk level indication consistent with existing patterns (e.g., ⚠️ warnings for dangerous operations).
- **Disclaimer** — the repository disclaimer ("This is not the official Forcepoint NGFW-SMC documentation...") appears at the top of README.md and must not be removed.

## Key Concepts for Content Accuracy

- **Active Server** — the primary management server; replication state `ACT_REP` when healthy.
- **Standby Server** — the secondary server; replication state `STB_REP` when healthy.
- **sgReplicationAdmin** — the single console script for all replication administration (except certification). Each execution produces a JSON report.
- **SMC API vs. GUI** — the GUI performs comprehensive pre-checks and composite actions; the SMC API and console commands do not. This distinction must be preserved accurately in any documentation.
- **SMC Python** — wraps the SMC API entirely; the `HAManagement` class in `smc.core.ha_management` exposes all HA entry points.
