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
