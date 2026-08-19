# Release 4.8.1 (2026-08-19)

### Bug Fixes

* retry transient network failures on every Terminus fetch (cb3fb43)

# Release 4.8.0 (2026-08-19)

### Features

* pr-multidev: php-quality + route-smoke built into the PR multidev flow (4315800)

# Release 4.7.0 (2026-08-19)

### Features

* rc: route-smoke gates the RC security scan (bc20977)

# Release 4.6.0 (2026-08-19)

### Features

* failure-only Slack notification (slack_channel input) (1026990)
* reusable route-smoke workflow — authenticated key-route checks via drush uli (5b6e70e)

### Bug Fixes

* retry + report login-curl TLS failures, stage timing echoes (2e4f97d)
* load Pantheon SSH key — terminus drush runs over SSH, machine token is not enough (8deb8c2)
* surface drush uli stderr on failure, curl --max-time 30, job timeout-minutes 10 (227b6a5)

### Documentation

* add /admin/structure/menu to recommended smoke routes (08b6c01)

# Release 4.5.0 (2026-08-19)

### Features

* reusable PHP quality workflow — plugin class-load check + custom unit tests (87f266d)

### Bug Fixes

* block-scalar the skip notice — colon-space in a plain run scalar is a YAML syntax error (cd9e741)

# Release 4.4.3 (2026-08-18)

### Bug Fixes

* verify ZAP PII (10062) findings against live pages before the severity gate (523d079)

# Release 4.4.2 (2026-08-18)

### Bug Fixes

* install release tooling in an isolated prefix (f4bfee5)

# Release 4.4.1 (2026-08-18)

### Bug Fixes

* ignore workspaces when installing semantic-release tooling (7e2a97f)

# Release 4.4.0 (2026-08-18)

### Features

* maintain moving major-version tags (v4, v5, …) on release (c504b13)

# Release 4.3.2 (2026-08-18)

### Bug Fixes

* gate npm ci on package-lock.json, not package.json (825af59)

# Release 4.3.1 (2026-08-11)

### Bug Fixes

* pin internal refs to v4.3.0 so the settle gates are reachable (e609935)

# Release 4.3.0 (2026-08-11)

### Features

* gate drush deploy on platform workflows, tree consistency, and files-mount readiness (f756cee)

### Bug Fixes

* raise settle-gate ceilings to 6min for large sites (81a519e)

# Release 4.2.3 (2026-08-11)

### Bug Fixes

* bump internal self-ref pins so shipped fixes actually run (529093f)

# Release 4.2.2 (2026-08-11)

### Bug Fixes

* bump node20-era action pins to node24-ready releases (69eaf3a)

# Release 4.2.1 (2026-07-18)

### Bug Fixes

* VRT reusable ignores the --run-vrt label its callers accept (0efd3ac)

# Release 4.2.0 (2026-07-18)

### Features

* double-hyphen label convention (--run-vrt et al) for picker grouping (91904ad)
* label-based opt-ins for VRT, security scan, and multidev skip flags (fb7eda0)

# Release 4.1.4 (2026-07-18)

### Bug Fixes

* RC multidev VRT opt-in never fires on merged-PR triggers (17b9783)

# Release 4.1.3 (2026-07-11)

### Bug Fixes

* pantheon: bump internal composite-action refs to v4.1.2 (1bd4fce)

# Release 4.1.2 (2026-07-11)

### Bug Fixes

* pantheon: wait for pushed commit to reach env filesystem before drush deploy (a9de69c)

# Release 4.1.1 (2026-07-01)

### Bug Fixes

* vrt: pin js-yaml to v4 and use named import (9b6c2ee)

# Release 4.1.0 (2026-06-16)

### Features

* add semantic-release workflow (6058d2d)

### Bug Fixes

* add /vrt/ prefix to S3 key paths in reusable-pantheon-vrt (b753308)
* migrate reusable-satis-publish to 1Password secrets (53eb8d8)
