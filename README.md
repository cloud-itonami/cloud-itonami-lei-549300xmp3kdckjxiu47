# cloud-itonami-lei-549300xmp3kdckjxiu47

> **Independent third-party archive/analysis. Not affiliated with, endorsed by, or sponsored by Marsh & McLennan Companies, Inc.**

This repository archives the publicly published Website Terms and Conditions of Use of
**Marsh & McLennan Companies, Inc.**, with source-url and retrieval-date provenance, per
[ADR-2607110300](https://github.com/com-junkawasaki/root/blob/main/90-docs/adr/2607110300-cloud-itonami-lei-corporate-tos-catalog.md)
(`cloud-itonami-lei-corporate-tos-catalog`, `com-junkawasaki/root`). It is a read-only
reference/archive repository — it does not act, propose, or execute anything on the
company's behalf, and is not a governed Advisor/Governor actor.

## Company identity

- **Legal name**: Marsh & McLennan Companies, Inc.
- **LEI (ISO 17442)**: [549300XMP3KDCKJXIU47](https://search.gleif.org/#/record/549300XMP3KDCKJXIU47) (GLEIF-verified)
- **Jurisdiction**: US-DE
- **Website**: https://www.marshmclennan.com
- **Ticker**: MMC (NYSE)

## Contents

- `80-data/public/tos.journal.edn` — EDN quad-log of archived Terms of Use documents,
  each entry carrying `:tos/full-text`, `:tos/source-url`, `:tos/retrieved-at`,
  `:tos/sha256`, `:tos/doc-type`, and a `:tos/supersedes` chain for future revisions.
- `NOTICE` — copyright/attribution statement for the archived third-party text.
- `blueprint.edn` — machine-readable company identity record.
- `facts.edn` — 103 verified registry facts with per-fact provenance. **Generated** — see below.
- `scripts/verify-facts.cljk` — re-fetches every source `facts.edn` cites and fails if
  the live record disagrees. Vendored from `com-junkawasaki/root`
  (`scripts/lei-verify-facts.cljs`); fix issues in the canonical and re-vendor.

## Verifying the record

The identity table above used to be assertions with nothing in the repository
behind them. `facts.edn` now carries them as data, and every value in it was read
out of a public registry response whose URL and retrieval time sit next to the
value:

```
nbb scripts/verify-facts.cljk           # check the recorded facts against the live sources
nbb scripts/verify-facts.cljk --write   # re-fetch and rewrite facts.edn
```

Seventeen GLEIF/ISO URLs were fetched and 103 facts recorded — the LEI record
(legal name as GLEIF spells it, **`MARSH & MCLENNAN COMPANIES, INC.`**, upper
case, `en`; entity **ACTIVE**, registration **ISSUED**,
**`FULLY_CORROBORATED`** / `CONFORMING`; two different addresses recorded
separately — the legal address `1209 ORANGE ST, 19801, WILMINGTON, US-DE, US`,
which is a registered-agent address in Delaware, and the headquarters
`1166 Avenue of the Americas, 10036, New York, US-NY, US`; entity creation date
`1969-03-17` — the Delaware holding-company incorporation, not the 1871/1905
origins of the Marsh & McLennan brokerage the company's own history describes,
which predate the entity that holds this LEI; initial LEI registration
`2014-01-07`, last updated `2025-12-05`, next renewal `2026-12-16`; no BIC — an
insurance broker, not a bank; OpenCorporates id `us_de/706226`, S&P Global id
`473342`), its ISIN mapping (**25** instrument identifiers — a count read from
`meta.pagination.total` of the cited page, counted but not mirrored), its
managing LOU and LEI-issuer accreditation (**Ubisecure Oy** / RapidLEI, FI,
accredited 2018-04-03 — the LEI's initial registration in 2014 predates this
LOU's accreditation by four years, so the record was necessarily issued
elsewhere and transferred to RapidLEI later; the transfer itself is not in the
record), registration authority `RA000602` (**Division of Corporations,
Department of State** — `corp.delaware.gov`, serving Delaware only — where the
entity is file number `706226`), ISO 20275 legal form `XTIQ`
(**`Corporation`**, US-DE), and **both consolidation levels**: no parent at
either level, each level carried as a reporting-exception entity with category
`DIRECT_ACCOUNTING_CONSOLIDATION_PARENT` /
`ULTIMATE_ACCOUNTING_CONSOLIDATION_PARENT` and reason **`NO_KNOWN_PERSON`** —
GLEIF's code for "no person consolidates this entity", i.e. MMC is itself the
top of its accounting group. Fifteen of the seventeen URLs answered `200`; the
`direct-parent` and `ultimate-parent` endpoints answered `404` because GLEIF
publishes the *exception* side of that pair for this entity, which the checker
treats as a fact rather than a failure.

The children are the finding of this record. GLEIF records **94 direct
children**, each mirrored below as its own `:fact/kind :direct-child` entity
with name, LEI and jurisdiction — and their distribution is `GB 28, US-DE 8,
NL 6, DE 5, ES 5, IE 5, FR 4, BE 4, …`: a New York-headquartered, US-listed
group whose GLEIF-visible subsidiary graph is three-and-a-half times deeper in
the United Kingdom than in Delaware. That is not the shape of the group — the
company's 10-K subsidiary exhibit names far more entities than 94 — it is the
shape of *LEI regulation*: a subsidiary appears here only if it holds an LEI
and reports the relationship, and EU/UK derivatives and insurance-distribution
rules compel that where US rules mostly do not. The 94 is a measured count of
what GLEIF's relationship graph holds, not a census of subsidiaries, and the
README says so precisely because the difference is easy to misread.

The check has three exit codes, not two: `0` when every cited URL answered and
every recorded value still matches, `1` when a citation broke or a value
drifted (each difference is named, with the recorded and live values side by
side), and `3` when the check could not be performed at all — `facts.edn`
missing or empty, or GLEIF unreachable at the transport level — because a check
that could not run must not look like a check that ran and found nothing.
Before this landed, all three were shown against the live API: unmodified →
`0` (`OK all 103 recorded fact(s) still match the live sources`);
`:company/jurisdiction` values edited `US-DE` → `FR` → `1`, naming each
affected entity and `:company/jurisdiction` as `DRIFT`; the
`gleif-direct-children-count` entity deleted → `1`, naming it as `ADDED`;
`facts.edn` absent → `3` (`INCONCLUSIVE … Refusing to report a pass`). Each
break was diffed against a backup before its run was trusted, and the file was
restored byte-identical afterwards (`shasum` equal).

## Design rationale

See ADR-2607110300 in `com-junkawasaki/root` (`90-docs/adr/`) for why this repo exists,
why it is keyed by LEI rather than GTIN or ticker, and why full-text archival (with
provenance) was chosen over excerpt-only storage.
