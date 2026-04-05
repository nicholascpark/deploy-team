# Invisilink Changelog

## v1.2 — 2026-03-28

- Renamed system from deploy-team to Invisilink
- Skill command renamed from `/deploy-team` to `/flash`
- Added `/gnosis` skill — establishes umbilical connection to parent for pre-existing repos
- Added `/bless` skill — parent-side delivery of `/gnosis` to pre-existing repos
- Added `download.md` backward-compatibility shim redirecting to `/flash`
- Linked ventures now pull upgrades directly from parent filesystem (no GitHub round-trip)
- Added v1.2 retrofit step to upgrade mode (writes `umbilical/config.md` + flash stub for existing incubator-spawned ventures)
- Gnosis registers directly in parent's ventures.md (no approval hook needed — parent already blessed)
- Trigger: manual (Invisilink rename + gnosis/bless implementation)

## v1.1 — 2026-03-27

- Expanded umbilical-monitor skill template with explicit signal type routing
- Added `parent_learning` signal type (medium priority, from parent's venture-learnings curation)
- Added `infrastructure_upgrade_available` signal type (low priority, invisilink version notifications)
- Added `assumption_challenge`, `market_intelligence`, `directive` signal types with routing rules
- Trigger: manual (federated learning system implementation)

## v1.0 — 2026-03-26

- Initial import from `nicholascpark/invisilink` GitHub repo
- Source of truth moved to parent repo (`tools/invisilink/flash.md`)
- GitHub repo becomes distribution mirror
- Trigger: manual (initial setup)
