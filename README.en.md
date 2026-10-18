# aut-level-checker

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Domain Profile](https://img.shields.io/badge/domain-aut%20%7C%20domain%2F1.0-blue.svg)](domain.manifest.json)
[![Status: RESERVED](https://img.shields.io/badge/status-RESERVED%20%7C%20open%20claim-orange.svg)](domain.manifest.json)
[![Standard: GB/T 40429](https://img.shields.io/badge/standard-GB%2FT%2040429-2ea44f.svg)](domain.manifest.json)
[![Privacy: default-off](https://img.shields.io/badge/privacy-default--off-brightgreen.svg)](privacy.json)

> **Status: RESERVED (placeholder · open for claim)** — this repository ships the four-piece industry-access skeleton already built to the platform protocol, and waits for an operator with the proper domain qualification to claim it and fill in the real rules.

It turns "which level is this system, and where does responsibility fall" into verifiable attributes: the AI only surfaces the level elements — it does **not** grade the system.

## What this domain covers

Determination of driving automation level (L0–L5) and the responsibility boundary. The automation level directly sets liability and the permissible marketing wording; mis-calling L2 as "autonomous driving" is a real-world hazard. The AI lays out the level elements and ODD; grade determination stays with the standard and the regulator.

## Quick start

This repo is a **domain-profile skeleton** (four JSON files), not a runnable application. Validate with standard JSON tooling:

```bash
git clone https://github.com/SynomosAI/aut-level-checker.git
cd aut-level-checker
python -m json.tool domain.manifest.json > /dev/null
python -m json.tool gate-map.json        > /dev/null
python -m json.tool privacy.json         > /dev/null
python -m json.tool checker.io.json      > /dev/null
```

The checker (MCP tool) IO contract is in `checker.io.json`. A minimal call:

```json
{
  "subject_ref": "mpp:cn:2a7b9c4d",
  "attribute_keys": ["aut.automation_level", "aut.responsibility_boundary"],
  "evidence": ["tsa:2026-09-12#sha256:9f2a..."],
  "request_id": "req-20260912-0006"
}
```

> The checker has exactly one action — *show and verify*. It has **no "judge" option**: "autonomous driving", "driverless", "responsibility on the system", "L4 already compliant" are all on the `_forbidden_outputs` blacklist.

## Domain identity

| Item | Value |
|---|---|
| Domain ID | `aut` (globally unique; never reused) |
| Domain name | Driving · Automation level |
| Profile version | `domain/1.0` |
| Status | `RESERVED` |
| Placeholder since | 2026-09-12 |

## Attributes

| Key | Type | Meaning |
|---|---|---|
| `aut.automation_level` | enum | Per GB/T 40429 · L0/L1/L2/L3/L4/L5 |
| `aut.odd_scope` | string | Operational Design Domain: road type, speed range, weather, geofence |
| `aut.takeover_capability` | enum | Takeover & minimal-risk strategy · not_applicable/driver_supervised/fallback_ready/no_driver_needed |
| `aut.responsibility_boundary` | enum | Responsibility boundary, human-verified · driver/system/shared/undetermined |

## Red lines (machine-readable)

1. Must not output an automation level higher than measured or unconfirmed.
2. Must not describe L0–L2 as "autonomous driving" / "driverless" wording.
3. Responsibility-boundary determination requires human review; the AI must not alone assign liability.

Each red line has a corresponding blocking rule in `gate-map.json`.

## The four-piece access set

| File | Role |
|---|---|
| `domain.manifest.json` | Domain declaration: attribute list, issuer requirements, validity, red lines |
| `gate-map.json` | Friction per action: silent / warn / confirm / block / require-owner |
| `privacy.json` | Privacy declaration: default-off, minimal, withdrawable, deletable |
| `checker.io.json` | Checker (MCP tool) IO contract — show & verify, never judge |

## References

GB/T 40429 Taxonomy of driving automation for vehicles; management norms for road testing and demonstration of intelligent connected vehicles. References only explain the source of attributes; they do not constitute legal advice.

## How to claim

1. Fork this repo and fill in `operator` and `checker_endpoint`.
2. Calibrate attribute values and red-line wording per currently effective rules.
3. Run the platform-side validator self-check (all five criteria must pass).
4. Open a PR with qualification evidence and the rule basis.

## License / 署名

Code and configurations are used under the MIT licence. Document copyright belongs to SynomosAI.

© 2026 SynomosAI. All rights reserved.
