# Rootless Docker File Ownership Helper

Reclaim file ownership back to user who starts a rootless container (1000), from the container operating User (typically 100998, 100000 ...)

```bash
#!/bin/bash

# Reclaims permissions back to the invoking user (from rootless docker 100000+ UID operating users)
# You can also: sudo chown ${USER}:${USER} -R .
# Great for git commits and backups.
# This script can be removed one day if rootless docker handles it, or if we switch to podman (slower than docker).

set -x
nsenter -U --preserve-credentials -n -m -t $(cat $XDG_RUNTIME_DIR/docker.pid) /usr/bin/chown -R root:root $(pwd)
```
