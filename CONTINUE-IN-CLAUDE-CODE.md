# Continue the FalconExam build in Claude Code

The specification kit is reviewed, consistent, and the three scope decisions are settled. This note is the handoff for building the actual product in Claude Code.

## Start a session

From this folder:

```powershell
cd "G:\My Drive\falconexam\falconexam-claude-development-kit\falconexam-development-kit"
claude
```

## How to run the build (one phase per session)

Do not ask Claude Code to implement everything at once. Work milestone by milestone.

1. Give it `prompts/00-master-architect.md` first — it produces repo scaffolding, architecture overview, Mermaid diagrams, threat-model skeleton, data-flow inventory, Docker Compose, CI skeleton, and the milestone roadmap. It hands off to Phase 1 (it does not implement domain modules itself).
2. Then run the phase prompts in order, each in its own session:
   - `prompts/01-foundation-and-domain.md`
   - `prompts/02-lti-and-lms-integrations.md`
   - `prompts/03-proctoring-and-media.md`
   - `prompts/04-ai-tensorflow-biber.md`
   - `prompts/05-privacy-security-retention.md`
   - `prompts/06-google-cloud-marketplace.md`
   - `prompts/07-testing-release-and-documentation.md`
3. After each accepted milestone, have it update `docs/STATUS.md`, `docs/DECISIONS.md`, and `docs/KNOWN_LIMITATIONS.md`, verify `specs/acceptance-checklist.md`, and make a Git commit.

The reusable per-milestone instruction is in `templates/CLAUDE-MILESTONE-INSTRUCTION.md`; new decisions use `templates/ADR-TEMPLATE.md`.

## Read these first — settled decisions (do not re-litigate)

`specs/` is authoritative. The following are already decided and recorded as ADRs under `decisions/`:

- **ADR-001 — Proctoring overlay, not an assessment engine.** The LMS owns exam content, timing, and grading. AGS is optional/off by default and writes back only a non-grade status or review link.
- **ADR-002 — Dedicated OIDC IdP for non-LMS staff.** Per-tenant SSO federation, mandatory MFA for privileged roles, audited/time-boxed support impersonation. Students/instructors stay on LTI.
- **ADR-003 — Unified entitlement model.** Marketplace and direct sales converge on one `Entitlement` → `SubscriptionPlan` → `CapacityQuota`; direct contracts are admin-provisioned through an audited workflow.

These are also summarized in the "Resolved scope decisions" section of `specs/product-requirements.md`.

## Context on the kit itself

`KIT-REVIEW.md` records the full review — every gap/inconsistency found and how it was resolved (missing entities added, roles reconciled, per-phase acceptance criteria added, platform requirements like i18n/browser matrix/notifications). Useful if you wonder why a spec reads the way it does.

## Publishing

`PUSH-TO-GITHUB.md` covers pushing to github.com/selvasmallive (already done). `.gitignore` is in place and blocks secrets/keys — keep it that way; never commit `.env`, service-account JSON, or `*.key`/`*.pem`.
