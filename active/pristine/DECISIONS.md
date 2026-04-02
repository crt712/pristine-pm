# DECISIONS


## Decisions Needed
- (none)

## Key Decisions
- Use GitHub for version control (Replit gitsafe-backup + optional external push)
- Deploy via Replit
- Token-based authentication
- Pristine Founder view (and any integration that loads founder markdown): read **`active/pristine/FOUNDERS_CHEAT_SHEET.md`** only; **`active/pristine/FOUNDER_BRIEF.md`** is not a programmatic source of truth (deprecated for reads)
- **[2026-04-01] Replit is the sole operating environment.** Cursor is no longer part of the operating model. This Replit-backed repo is the only source of truth. No parallel development in external editors. No Cursor dependency for review, migration, or repo truth. See `docs/OPERATING_MODEL.md`.

## Architecture Notes
- Main branch: production
- Canon path: `repo/active/pristine/` (git-enabled mode)
- Replit is sole operating environment — no Cursor IDE integration
