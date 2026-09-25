# How to read this data

This dataset is the record of what an internet-facing honeypot was *sent*. Every
row is something an attacker did to a machine that was pretending to be
vulnerable. That framing matters, because it is easy to read a honeypot feed and
claim far more than it supports. This page is about reading it honestly.

## The one rule

Move in this order and never skip a step:

**Observed fact → technical interpretation → confidence → investigation hypothesis.**

- **Observed fact** — what is literally in the data. *"203.0.113.9 sent
  `root/admin` to the SSH port 412 times in 90 seconds."*
- **Technical interpretation** — what that behaviour *is*, in neutral terms.
  *"A credential brute-force against SSH."*
- **Confidence** — how sure the interpretation is, and why. *"High: the rate and
  the wordlist are characteristic; low ambiguity."*
- **Investigation hypothesis** — what you would check next to learn more, phrased
  as a question, not a conclusion. *"Does this address also appear in other feeds
  as a known SSH brute-forcer?"*

The failure mode is jumping straight to a story — "a threat actor is targeting
us" — from a fact that only supports "a bot tried common passwords". This feed
supports the second sentence, rarely the first.

## The behaviour ladder

Traffic to a honeypot sorts into a small number of kinds. They escalate in
intent, and most of the internet never leaves the first two rungs.

| Behaviour | What it looks like in this data | What it means | What it does *not* mean |
|---|---|---|---|
| **Background noise** | A handful of connections, no login, or a single banner grab across many ports | Untargeted internet-wide measurement (researchers, census scanners, worms looking for a handshake) | Not an attack on *you*; the same packets hit every address |
| **Scanning** | One source touching many ports, or one port across a short window; protocol probes on the wrong port | Reconnaissance — mapping what answers | Interest in you specifically; scanners hit whole ranges |
| **Brute-force** | Many auth attempts to one service, common username/password pairs (see `credentials/`) | Automated credential guessing | A compromise — the credentials are *tried*, not *known to work* |
| **Exploitation** | A specific request matching a known CVE or a command injection payload (see `incidents/` and `iocs/`) | An attempt to run code or gain access via a named weakness | That it *succeeded* — the service was emulated; nothing was actually exploited |
| **Post-exploitation behaviour** | Commands run *after* a shell is offered: downloading a second stage, adding persistence, killing rivals, reading system files | The playbook a bot runs once it thinks it is in | That any real system was compromised — the "shell" was a decoy that recorded the script |

The last row is the valuable one: it is the attacker's own automation, captured.
But read it for what it is — *what the bot would have done* — not as evidence
that it did it to a real host. **No real service was ever run, and no real
system was compromised to produce this data.**

## Reading each artefact

- **`iocs/source-ips.csv`** — the addresses, with a summary of what each sent. An
  address here is one that behaved maliciously *toward the sensor*. Shared
  hosting, NAT and proxies mean one address can be more than one actor, and a
  reassigned address can be innocent later. Treat it as "seen doing X on this
  date", not as a permanent verdict on whoever holds the address now.
- **`credentials/attempted-credentials.csv`** — pairs that were *tried*. This is a
  view of what botnets currently guess, which is useful for building deny-lists
  and for spotting when your own defaults are in the corpus. It is not a list of
  passwords that worked anywhere.
- **`iocs/payload-urls.csv`** and **`iocs/sample-hashes.csv`** — where attackers
  tried to pull a second stage from, and the hashes of what was captured. Look
  these up in the usual reputation sources; correlate, don't conclude.
- **`incidents/`** — a readable account of one source's activity. These describe
  technique and sequence. They deliberately stop short of attribution.

## Confidence, honestly

Attach a confidence level and be willing to say "low".

- **High** — the behaviour is unambiguous and self-evidencing: a payload that
  matches one specific CVE, a login rate no human produces.
- **Medium** — consistent with a technique but with benign explanations: a single
  odd request could be a broken client as easily as a probe.
- **Low / inconclusive** — say so and stop. A feed like this has a long tail of
  traffic that is simply unexplained, and inventing a reason for it is the most
  common way these datasets get misread.

## What this data cannot tell you

- **Who.** Source addresses are not identities. This dataset makes **no
  attribution claims** — not to a person, a group, or a country — and neither
  should anything built from it. Geolocation is where an address is *routed*, not
  where an operator sits.
- **Why you.** Almost none of this is aimed at anyone specifically. A honeypot
  sees the same broad, automated hostility every public address sees.
- **That anything was breached.** The whole point of the sensor is that the
  services were fake. "An exploit was attempted" is a fact; "a system was
  compromised" is a different claim that this data, on its own, does not make.

## Using it to check your own systems

The honest, useful workflow:

1. Take the indicators (addresses, credential pairs, sample hashes, payload URLs).
2. Check whether any appear **in your own logs**.
3. If they do, that is a fact worth an *investigation hypothesis* on your side —
   begin the ladder again with your evidence, not this one's.

The value of a honeypot feed is as a set of things to *look for* in ground truth
you control, not as a set of conclusions to *import*.

---

*Generated 2026-09-25 18:14 UTC.*
