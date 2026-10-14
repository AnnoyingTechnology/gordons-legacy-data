# Gordon's Legacy data

Component data for [Gordon's Legacy](https://github.com/AnnoyingTechnology/gordons-legacy), the open
interior-ballistics simulator carrying on Gordon's Reloading Tool: powders, projectiles and calibers, one
JSON file each, under CC0 1.0.

**This is not load data.** A powder model predicts pressures that may be wrong; every charge must come
from the powder or bullet maker's published data, started low and worked up.

## Layout

```
powders/       one file per powder model
projectiles/   one file per projectile
calibers/      one file per caliber definition (case, chamber, rifling)
index.json     every file with its SHA-256, read by the app
```

Each file:

```json
{
  "format": "gordons-legacy.component",
  "version": 1,
  "kind": "powder | projectile | caliber",
  "provenance": { "origin", "source", "file", "author", "date", "method", "status" },
  "value": { ... }
}
```

`value` follows the load format of Gordon's Legacy (`Propellant`, `Projectile`, `CaliberDefinition` in
[`packages/formats/src/load/`](https://github.com/AnnoyingTechnology/gordons-legacy/tree/main/packages/formats/src/load)):
SI base units (m, kg, s, Pa, K, J/kg), every field documented there.

## Status and intake

The first files are the GRT community's shared files ([zen/grt_databases](https://github.com/zen/grt_databases)
at `9d2f84d`, CC0), converted to SI by `tools/data/export-repository.ts` in the main repository. Their
status is `unverified`: nobody has checked them against published data yet.

Intake rules (main repository, `docs/PLAN.md`, Data):

- Accepted: CC0 community files (unverified until checked), models fitted from published charge,
  velocity and pressure data, factual specifications (bullet weight, length, BC; cartridge dimensions).
- Not accepted: GRT's database or anything derived from it; whole tables copied from manufacturers'
  manuals.
- Data changes are safety-critical: each one needs the maintainer's review.
