# UPSTREAM_TRACKING_POLICY v1.0 — DESIGN FREEZE ARTIFACT

> **STATUS: DESIGN ONLY / FROZEN IN EVIDENCE.**
> NOT committed to any repository. NO repository mutation authorized or performed.
> Can become a controlled artifact in the public repo ONLY via a separate, later authorization gate.
>
> Policy model: **MODEL C** — observation and integration source are separate concepts.
> SHA-256: computed AFTER final write as EXTERNAL evidence — not embedded in this file.

---

## 0. Document provenance
- Type: local design artifact (frozen in evidence).
- Repositories: fork `KrzysiekSko/diegosouzapw-OmniRoute`; upstream `diegosouzapw/OmniRoute`.
- Evidence basis: live GitHub readback + local read-only git graph analysis, 2026-09-15.
- Governance truth = LIVE_REPOSITORY_STATE + LIVE_GITHUB_METADATA + COMMIT_HISTORY + FILE_CONTENT + VERIFIABLE_EVIDENCE. **Agent memory is NOT governance evidence.**

---

## Preamble — consolidated verified graph (frozen evidence)

| Ref | HEAD | Note |
|---|---|---|
| fork/main | `05424cbd73474dbb4bd92c678352901ef2966961` | canonical fork baseline |
| upstream/main | `a3ca33fa6442b59adc42976c795709eaf5351109` | non-default, fallback ref |
| upstream/release/v3.8.51 (upstream default) | `df87e9363b6b08bd6b40ba9b774753b98d5a2cd4` | active release line |

- Merge-base (fork/main ↔ upstream/main) = merge-base (fork/main ↔ upstream/release) = `8e383f5c…` (2026-07-17).
- fork/main ↔ upstream/main: **ahead 4 / behind 45**.
- fork/main ↔ upstream/release: **ahead 4 / behind 4026**.
- 4 fork-unique commits = fork identity work (`#1`–`#4`; `#1`/#2 manifest add+revert, `#3` bilingual identity, `#4` README notice).
- Fork's last included upstream release: **v3.8.48** (identical 2026-07-17 lineage); v3.8.49/v3.8.50 NOT in fork lineage.

**Interpretation rule (critical):** the 4026-commit gap to `upstream/release/v3.8.51` is a property of an active release line, NOT a backlog of changes to "catch up". Integration is evaluated as a concrete release/change-set and its **effective diff relative to our baseline** — never as mechanical catch-up.

---

## 1. Purpose
Define how the controlled fork tracks upstream `diegosouzapw/OmniRoute`, classifies its changes, and selectively integrates verified releases/change-sets into the controlled integration baseline `main`, such that upstream is never a trusted write source and no change reaches `main` without explicit operator authorization.

## 2. Scope
Applies to upstream change detection, classification, candidate creation, validation, PR, merge, post-merge verification, and escalation. Does NOT cover runtime/config changes, publishing, or Hermes runtime integration (separate gates, currently NOT_AUTHORIZED).

## 3. Definitions
- **UPSTREAM** = `diegosouzapw/OmniRoute` (TRACKED_CHANGE_SOURCE).
- **MAIN** = fork `main` (CONTROLLED_INTEGRATION_BASELINE).
- **DISCOVERY_REF** = dynamic `upstream/<CURRENT_DEFAULT_BRANCH>`; OBSERVATION_ONLY.
- **INTEGRATION_REF** = an operator-selected, verified release or tag; CANDIDATE_SOURCE.
- **FALLBACK_REFERENCE** = `upstream/main`.
- **candidate/upstream-<version>** = ephemeral integration branch.

## 4. Trust model (Model C)
```
UPSTREAM ≠ TRUSTED WRITE SOURCE
UPSTREAM = TRACKED CHANGE SOURCE
MAIN     = CONTROLLED INTEGRATION BASELINE

DISCOVERY_REF     = observation only
INTEGRATION_REF   = candidate source (operator-verified release/tag)
MARRIAGE OF THESE = forbidden by default

AUTOMATIC_TRACKING_REF_PROMOTION = NO
AUTOMATIC_INTEGRATION            = NO
AUTOMATIC_MERGE                  = NO
```
No upstream ref, tag, release, or default-branch change ever auto-mutates `main`. The discovery ref being upstream's current default does NOT make it an integration source without explicit verification and authorization.

## 5. Tracking sources (read-only)
1. Upstream current default branch head (DISCOVERY_REF — observation only).
2. Upstream `main` (FALLBACK_REFERENCE).
3. Upstream release tags `v3.8.*` and GitHub Releases (`released` type).
4. Security signals: upstream CodeQL/semgrep/scorecard runs, security advisories, Dependabot PRs.
5. `CHANGELOG.md` for release intent (aggregated changelog present upstream).

## 6. Tracking cadence
- Read-only polling of DISCOVERY_REF (proposed daily; cadence is an automation/scheduling decision for a later gate).
- Compare against a **stored last-known tracking SHA** ("last known good"), never "now vs upstream".

## 7. Change detection
Detect: new head on default branch; new release tag; new GitHub Release; upstream default-branch change; force-push/tag-move on watched refs; new dependency CVE; new advisory. Compute delta commit-set since last-known SHA. Tag/release does NOT automatically become an integration candidate — it must first be verified as an appropriate source artifact.

## 8. Classification
Categories: `SECURITY | BUGFIX | FEATURE | DEPENDENCY | BREAKING | CI | DOCUMENTATION | RELEASE | UNKNOWN`. Classify from commit/file evidence (auth/security paths ⇒ SECURITY; manifest/lockfile ⇒ DEPENDENCY; API/schema ⇒ BREAKING), not message keywords alone. UNKNOWN escalates for operator review.

## 9. Alert priorities
```
P0 = critical security / actively exploitable (auth, RCE, secret leak)
P1 = important security / severe operational defect
P2 = normal bugfix / compatibility / dependency
P3 = feature / optimization
P4 = documentation / low-risk maintenance
```
**ALERT_PRIORITY ≠ MERGE_AUTHORIZATION.** Even P0 raises a notification + expedited evaluation, NEVER auto-merge.

## 10. Candidate branch policy
- Name: `candidate/upstream-<version>` (e.g. `candidate/upstream-v3.8.51`), else `candidate/upstream-YYYYMMDD-<shortsha>`.
- Attributes: `SOURCE_COMMIT`, `SOURCE_TAG_OR_RELEASE`, `BASE_FORK_SHA`, `CREATION_AUTHORIZATION` (gate UTP-04), `LIFECYCLE`, `EXPIRATION/CLEANUP`.
- Candidate creation NOT authorized by this document.

## 11. Integration strategy
Per-change decision (no universal rule):
- **MERGE**: coherent release/change-set integrable while preserving fork-specific mods (FORK.md, PL FORK.md, README notice).
- **CHERRY-PICK**: isolated fix (esp. security) with clean deps.
- **MANUAL PORT**: upstream architecture conflicts with fork control-plane changes.
- **REIMPLEMENTATION**: behavior kept but upstream impl incompatible with fork architecture/governance.
- **SKIP**: irrelevant/harmful/obsolete/conflicting (documented rationale required).

## 12. Validation requirements (minimal path to merge eligibility)
| Check | Status |
|---|---|
| SOURCE_PROVENANCE | MANDATORY (verify upstream SHA from real upstream remote) |
| DIFF_REVIEW | MANDATORY (effective diff vs our baseline, not raw catch-up) |
| FORK_IDENTITY_PRESERVATION | **MANDATORY** (FORK.md, PL FORK.md, README notice intact) |
| ROLLBACK_PLAN | MANDATORY |
| DEPENDENCY_REVIEW | MANDATORY |
| LICENSE_CHECK | CONDITIONAL (MIT) |
| BUILD / LINT / TYPECHECK / UNIT / INTEGRATION | CONDITIONAL (upstream has these; fork-side wiring NOT_CURRENTLY_AVAILABLE) |
| SECURITY_SCAN | CONDITIONAL / NOT_CURRENTLY_AVAILABLE in fork |
| HERMES_COMPATIBILITY / OMNIROUTE_RUNTIME_COMPATIBILITY | NOT_CURRENTLY_AVAILABLE (runtime integration not authorized) |
| CONFIG_MIGRATION_IMPACT | CONDITIONAL |

## 13. Authorization gates (UTP-01 … UTP-09)
```
UTP-01 UPSTREAM_CHANGE_DETECTED
UTP-02 CHANGE_CLASSIFIED
UTP-03 INTEGRATION_CANDIDATE_PROPOSED
UTP-04 CANDIDATE_CREATION_AUTHORIZED
UTP-05 CANDIDATE_VALIDATED
UTP-06 PR_CREATED
UTP-07 PR_VERIFIED
UTP-08 MERGE_AUTHORIZED
UTP-09 POST_MERGE_VERIFIED
```
Separation: no stage auto-authorizes a later mutation. DETECTION/CLASSIFICATION/PROPOSAL are read-only; CANDIDATE_CREATION (UTP-04) and PR (UTP-06) are explicit mutation gates; MERGE (UTP-08) is the most explicit.

## 14. Merge policy
No auto-merge. Merge only after UTP-08 operator authorization, to `main` only, via PR, preserving fork identity. Do not delete source branch without separate authorization.

## 15. Post-merge verification
Independent readback: `main` head; default still `main`; FORK.md + PL FORK.md + README notice present; PR MERGED/CLOSED; effective delta == authorized delta; no unexpected files.

## 16. Upstream default-branch changes
```
UPSTREAM_DEFAULT_BRANCH_CHANGED
→ ALERT (P1)
→ DISCOVERY (role of new branch)
→ ROLE_VERIFICATION
→ OPERATOR_DECISION
```
Never auto-promote/redefine our tracking baseline solely because upstream changes its GitHub default branch.

## 17. Security emergency handling
P0/P1 security change: immediate alert (via approved notification channel), expedited classification/evaluation. "Expedite" ≠ auto-merge; hot-cherry-pick may prepare a separate branch for review, but nothing reaches `main` without authorization (UTP-04 → UTP-08).

## 18. Rollback principles
Every integration revertible; record merge SHA and prior state. On regression, revert via governed path (new PR + auth), never force-push/history-rewrite `main`.

## 19. Evidence requirements
Every integration stage records live GH evidence (refs, SHAs, PR number, diff stats, validation results, operator decision). Agent memory is NOT evidence. No credentials/tokens disclosed.

## 20. Automation boundaries
**Auto (read-only/diagnostic, no approval):** read-only polling; release/tag/default-branch/advisory/security-scan detection; classification + priority proposal; diff generation; impact report; test execution on an already-authorized candidate; alert generation.
**Always require explicit authorization (mutations):** candidate branch creation, commit, push, PR creation, merge into `main`, any `main` modification, branch deletion, tag/release creation, config/runtime changes, upstream sync.

## 21. Exceptions
Only with documented operator decision + written approval. No ad-hoc exceptions.

## 22. Policy review cadence
Review on each fork major baseline change, on material upstream topology change, or quarterly, whichever first.

---

## Machine-readable state model (for later automation, NOT written to repo)

```yaml
upstream_tracking:
  model: MODEL_C
  source_repository: diegosouzapw/OmniRoute
  discovery_ref: upstream/<CURRENT_DEFAULT_BRANCH>   # OBSERVATION_ONLY
  integration_ref: OPERATOR_SELECTED_VERIFIED_RELEASE_OR_TAG  # CANDIDATE_SOURCE
  fallback_reference: upstream/main
  automatic_tracking_ref_promotion: NO
  automatic_integration: NO
  automatic_merge: NO

  detection:
    status: NO_CHANGE | DETECTED | UNKNOWN
    last_known_sha: ""
    current_default_branch: ""
    upstream_default_changed: false

  classification:
    category: SECURITY | BUGFIX | FEATURE | DEPENDENCY | BREAKING | CI | DOCUMENTATION | RELEASE | UNKNOWN
    priority: P0 | P1 | P2 | P3 | P4
    rationale: ""

  integration:
    status: NOT_PROPOSED | PROPOSED | AUTHORIZED | VALIDATING | PR_READY | MERGE_PENDING | VERIFIED_CLOSED | SKIPPED
    candidate_branch: "candidate/upstream-<version>"
    strategy: MERGE | CHERRY_PICK | MANUAL_PORT | REIMPLEMENTATION | SKIP
    source_tag_or_release: ""

  authorization:
    candidate_creation: NO
    pr_creation: NO
    merge: NO

  rollback:
    merge_sha: ""
    prior_main_sha: ""
    revert_ready: false
```

---

## OPEN QUESTIONS / EVIDENCE GAPS
1. Fork-side CI capability (runners/tests) — NOT_CURRENTLY_AVAILABLE.
2. CodeQL actual continuous coverage — unverified (upstream comments suggest manual until configured).
3. Which release/tag will be the first verified INTEGRATION_REF — an operator decision at that future gate.
4. npm/Docker publish-identity collision — a governance constraint to formalize before any publishing work (fork has no release workflow installed).
5. Exact cadence/polling scheduling for detection — a later automation decision.

---

## FREEZE FOOTER
- Independent consistency readback performed (structure, gates 1–22 present, Model C, no missing section).
- SHA-256 of this artifact is recorded EXTERNALLY (after final write). This file does not contain its own hash.
- **STATUS = DESIGN_FROZEN_IN_EVIDENCE. POLICY_ACTIVE = NO. POLICY_ENFORCED = NO. REPOSITORY_MUTATION = NONE.**
- Next action: separate authorization gate decides whether this becomes a controlled artifact in the public repo.