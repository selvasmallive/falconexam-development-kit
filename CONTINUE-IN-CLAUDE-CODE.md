# Continue the FalconExam build in Claude Code

**The build has started.** This kit is the specification source; the product is built in a separate
repository. Milestone 0 (scaffolding) is complete.

## Where the product repository is

```
G:\My Drive\falconexam\falconexam
```

That repository vendors a copy of this kit at `development-kit/`, which is what makes the
`development-kit/specs` and `development-kit/prompts` paths inside the prompts and the milestone
template resolve. This kit repository stays authoritative for specification changes — if a spec
changes here, sync the copy there deliberately and note it in the product repo's
`docs/DECISIONS.md`.

## Start the next session

```powershell
cd "G:\My Drive\falconexam\falconexam"
claude
```

Read `docs/STATUS.md` first — it records exactly what exists and, importantly, what has and has not
been verified.

## Where the build is

| Milestone | Prompt | Status |
| --- | --- | --- |
| 0 — Scaffolding and architecture baseline | `00-master-architect.md` | **Complete** (2026-08-12) |
| 1 — Foundation, tenancy and domain | `01-foundation-and-domain.md` | **Next** |
| 2 — LTI 1.3 and LMS integrations | `02-lti-and-lms-integrations.md` | Not started |
| 3 — Proctoring, sessions and media | `03-proctoring-and-media.md` | Not started |
| 4 — TensorFlow and Biber AI | `04-ai-tensorflow-biber.md` | Not started |
| 5 — Privacy, security, retention | `05-privacy-security-retention.md` | Not started |
| 6 — Google Cloud and Marketplace | `06-google-cloud-marketplace.md` | Not started |
| 7 — Testing, release, documentation | `07-testing-release-and-documentation.md` | Not started |

Do not re-run `00-master-architect.md`. It produced the repository layout, architecture overview and
Mermaid diagrams, domain-boundary map, threat-model skeleton, data-flow inventory, Docker Compose
environment, CI skeleton and milestone roadmap, and handed off. The live roadmap is
`docs/ROADMAP.md` in the product repository.

## How to run each milestone

One milestone per session, in order. Give Claude Code
`development-kit/templates/CLAUDE-MILESTONE-INSTRUCTION.md` together with the phase prompt.

After each accepted milestone: update `docs/STATUS.md`, `docs/DECISIONS.md` and
`docs/KNOWN_LIMITATIONS.md`, update the threat model and data-flow inventory for anything new,
re-verify `specs/acceptance-checklist.md`, and commit. New decisions use `templates/ADR-TEMPLATE.md`.

## Before Milestone 1 — two things to hand over

1. **Start the local environment first.** `.\scripts\dev-up.ps1`. Milestone 0 was authored while
   Docker Desktop was not running, so the Compose file validates but has never actually been
   started. Confirming the PostgreSQL and Redis health checks is a Milestone 1 acceptance criterion
   anyway, and it verifies the scaffolding.
2. **Settle three open specification gaps.** They are recorded in
   `docs/architecture/assumptions-and-conflicts.md`. This kit requires the behaviour but never named
   an entity for any of them:
   - **G-01** — the student appeal/dispute workflow (Phase 5) has no `Appeal` entity.
   - **G-02** — identity-proofing results (Phase 5) have no `IdentityVerification` entity.
   - **G-03** — proctor–student chat (Phase 3) has its own retention schedule (Phase 5) but no
     `ChatMessage` entity.

   All three change the database schema. Deciding at Milestone 1 costs nothing; discovering them at
   Milestone 5 means migrating live student evidence.

## Read these first — settled decisions (do not re-litigate)

`specs/` is authoritative. The following are decided and recorded as ADRs under `decisions/` (and
mirrored into the product repo's `docs/adr/`):

- **ADR-001 — Proctoring overlay, not an assessment engine.** The LMS owns exam content, timing and
  grading. AGS is optional/off by default and writes back only a non-grade status or review link.
- **ADR-002 — Dedicated OIDC IdP for non-LMS staff.** Per-tenant SSO federation, mandatory MFA for
  privileged roles, audited/time-boxed support impersonation. Students/instructors stay on LTI.
- **ADR-003 — Unified entitlement model.** Marketplace and direct sales converge on one
  `Entitlement` → `SubscriptionPlan` → `CapacityQuota`; direct contracts are admin-provisioned
  through an audited workflow.
- **ADR-004 — Monorepo layout and toolchain** (added during Milestone 0, lives in the product repo).
  Gradle multi-project for `services/api`, pnpm + Node 22 for `apps/web`, `uv` for `services/ai`;
  modular monolith with boundaries enforced by ArchUnit rather than convention.

These are also summarized in the "Resolved scope decisions" section of `specs/product-requirements.md`.

## Context on the kit itself

`KIT-REVIEW.md` records the full review — every gap/inconsistency found and how it was resolved
(missing entities added, roles reconciled, per-phase acceptance criteria added, platform requirements
like i18n/browser matrix/notifications). Useful if you wonder why a spec reads the way it does.

## Publishing

`PUSH-TO-GITHUB.md` covers pushing this kit to github.com/selvasmallive (already done). **The product
repository has no remote yet** — it is committed locally only. The same instructions apply if you
want to publish it; use a different repository name, and keep it private.

`.gitignore` is in place in both repositories and blocks secrets/keys — keep it that way; never
commit `.env`, service-account JSON, or `*.key`/`*.pem`.
