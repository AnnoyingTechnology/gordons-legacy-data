# Gordon's Legacy data

Component data for [Gordon's Legacy](https://github.com/AnnoyingTechnology/gordons-legacy), the open
interior-ballistics simulator carrying on Gordon's Reloading Tool: GRT's own component catalogue
(powders, bullets, cartridges), the community's powders, projectiles and calibers, and the makers'
published gun specifications.

**This is not load data.** A powder model predicts pressures that may be wrong; every charge must come
from the powder or bullet maker's published data, started low and worked up.

## Layout

```
powders/       one file per powder model
projectiles/   one file per projectile
calibers/      one file per caliber definition (case, chamber, rifling)
firearms/      one file per maker, holding that maker's published guns
grt/           GRT's own database: powders/ and projectiles/ one file per maker, calibers/ one
               file per standard
index.json     every file with its SHA-256, read by the app
```

Each file:

```json
{
  "format": "gordons-legacy.component",
  "version": 1,
  "kind": "powder | projectile | caliber | firearm | grt-powders | grt-projectiles | grt-calibers",
  "provenance": { "origin", "source", "file", "author", "date", "method", "status" },
  "value": { ... }
}
```

A gun file's and a `grt-` file's `value` is a **list** rather than one entry: the application downloads a maker's whole
catalogue to search it, and a file per gun would make a first search thousands of requests.

`value` follows the load format of Gordon's Legacy (`Propellant`, `Projectile`, `CaliberDefinition` in
[`packages/formats/src/load/`](https://github.com/AnnoyingTechnology/gordons-legacy/tree/main/packages/formats/src/load)):
SI base units (m, kg, s, Pa, K, J/kg), every field documented there.

## The gun database

`firearms/` holds what the makers publish about their guns — the cartridge, the barrel length, the twist
and the weight — read from their own specification pages, one second between pages, with the page and the
date on every record. Nothing else is taken, and no maker's text is reproduced.

**A published figure is not a measurement of the gun in front of you.** A barrel length is nominal, and a
weight means nothing until you know what it includes: each record says so in `source.massBasis`
(`without-magazine`, `empty-magazine`, `loaded`, or `unstated` where the page did not say), and keeps the
maker's own strings in `source.published` so a rounding or a unit can be traced without the page. Measure
your own gun; the database is there to save typing, not to tell you what you own.

A gun that leaves a catalogue stays in these files. Discontinued guns are loaded for decades.

## Status and intake

The files outside `grt/` and `firearms/` are the GRT community's shared files ([zen/grt_databases](https://github.com/zen/grt_databases)
at `9d2f84d`, CC0), converted to SI by `tools/data/export-repository.ts` in the main repository. Their
status is `unverified`: nobody has checked them against published data yet.

Intake rules (main repository, `docs/PLAN.md`, Data):

- Accepted: CC0 community files (unverified until checked), models fitted from published charge,
  velocity and pressure data, factual specifications (bullet weight, length, BC; cartridge dimensions;
  a gun's barrel length, twist and weight).
- GRT's database: its own records, converted, under the family's permission (2026-11-04); not its
  encrypted file, nor entries a user of the copy made, nor powder models GRT hid.
- Not accepted: whole tables copied from manufacturers' manuals.
- Data changes are safety-critical: each one needs the maintainer's review. CI checks every change
  with the main repository's validator (`tools/data/validate-repository.ts`): index and files agree,
  hashes match, every file is a valid component. A green check is necessary, not sufficient.

## GRT's catalogue

`grt/` holds the cartridges, bullets and powder models of Gordon's Reloading Tool's own database
(GRT nightly 2030), converted to SI, published with the written permission of Gordon's family
(2026-11-04). That permission, not CC0, covers these files. They hold GRT's own entries only, none
GRT deleted, and no powder model GRT hid. Each entry keeps its GRT authorship in `info`. How the
database was read, and every record left out with the reason:
[docs/formats/grt-database.md](https://github.com/AnnoyingTechnology/gordons-legacy/blob/main/docs/formats/grt-database.md)
and [docs/data/grt-catalogue.md](https://github.com/AnnoyingTechnology/gordons-legacy/blob/main/docs/data/grt-catalogue.md).
GRT's powder models are its author's fits, not checked here against published data: their pressures
may be wrong.
