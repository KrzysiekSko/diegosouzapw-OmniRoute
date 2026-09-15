# OmniRoute Fork — Roadmap

This document describes the direction of this controlled fork of
[diegosouzapw/OmniRoute](https://github.com/diegosouzapw/OmniRoute).

It describes **capability maturity**, not release milestones. Phases indicate
logical ordering, not versions or dates. This roadmap is a public statement of
direction, not a commitment schedule, and does not imply that planned features
are implemented.

## Status legend

| Marker | Meaning |
| --- | --- |
| `[x]` Complete | implemented and verified |
| `[~]` In progress | active work, not yet complete |
| `[ ]` Planned | planned, not implemented |

---

## Phase 1 — Fork Foundation

- [x] Public fork identity — `FORK.md`
- [x] Bilingual fork documentation — `FORK.md` (EN) and `docs/i18n/pl/FORK.md` (PL)
- [x] Repository presentation metadata — description and topics

## Phase 2 — Governance

- [~] Upstream tracking policy — publication pending
- [ ] Release / versioning policy
- [ ] CI / security / supply-chain baseline

## Phase 3 — Hermes Integration

- [ ] OpenAI-compatible integration baseline
- [ ] Deterministic routing
- [ ] Verified combos
- [ ] Provider abstraction

## Phase 4 — AI Control Plane

- [ ] Routing policies
- [ ] Fallback / circuit breakers
- [ ] Quota and cost controls
- [ ] Telemetry / audit

## Phase 5 — Multi-Agent / Multi-Cell

- [ ] Dedicated OmniRoute per Hermes cell
- [ ] Isolation verification
- [ ] Controlled inter-agent communication

---

Status is maintained as the fork evolves. Items move from `[ ]` to `[x]` only
after the relevant governance gate is closed with verifiable evidence.