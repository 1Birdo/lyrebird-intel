# Lyrebird Honeypot — Threat Intelligence

Data captured by an internet-facing honeypot operated by
[birdo.uk](https://birdo.uk). Updated several times a day.

**[Browse the summary →](https://1birdo.github.io/lyrebird-intel/)** ·
[Weekly reports](reports/index.md) · [Latest week](reports/latest.md)

**This repository contains data only.** The honeypot itself — its emulation,
detection logic, deployment and tooling — is private and deliberately not
published here. Nothing in this repository reveals how the sensor is built or
where it runs.

## Contents

| path | what |
|---|---|
| `iocs/source-ips.csv` | 3,174 attacking addresses with evidence summary |
| `iocs/payload-urls.csv` | 580 URLs attackers tried to fetch malware from |
| `iocs/sample-hashes.csv` | 240 delivered samples, hashed |
| `credentials/attempted-credentials.csv` | 3,622 distinct username/password pairs |
| `samples/` | the captured binaries, zip-encrypted |
| `incidents/` | 1,507 per-address write-ups of reported attacks |

## Samples

Archives in `samples/` are zip-encrypted with the password `infected`, the
convention for distributing malware. This is not secrecy — it prevents accidental
execution and stops antivirus quarantining the whole checkout on clone.

**Everything here is hostile.** Analyse only in an isolated environment.

## What is recorded

Attacker input, verbatim: commands typed, credentials tried, paths requested,
user agents sent, payload URLs fetched.

**Not recorded here:** the honeypot's own responses. Publishing what the
emulation says back would let anyone build a detector for it, so the dataset is
deliberately one-sided.

## Provenance and caveats

Services are emulated — no real host was ever compromised, and no attack
succeeded against real infrastructure. Timestamps are UTC. Addresses are as
observed; no attempt is made to resolve shared hosting or NAT, so an address may
represent more than one actor.

Abuse reports derived from this data are filed as `birdo.uk` and link back to the
matching page under `incidents/`.

## Licence

Data is published for research and defensive use.

---

*Generated 2026-09-13 06:03 UTC.*
