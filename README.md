# Rootless Docker File Ownership Helper

To help people running into this blocker until Docker gets around to resolving this issue.

Reclaim file ownership back to user who starts a rootless container (1000), from the container operating User (typically 100998, 100000 ...)

### docker_reclaim.sh
```bash
#!/bin/bash

# Reclaims permissions back to the invoking user (from rootless docker 100000+ UID operating users)
# You can also: sudo chown ${USER}:${USER} -R .
# Great for git commits and backups.
# This script can be removed one day if rootless docker handles it, or if we switch to podman (slower than docker as of this writing).

set -x
nsenter -U --preserve-credentials -n -m -t $(cat $XDG_RUNTIME_DIR/docker.pid) /usr/bin/chown -R root:root $(pwd)
```

### Friendly reminders
* root from within the container namespace refers to the non-root invoking User. 
* Rootless Docker currently does not handle this as of this writing (March 2022).

### References
* https://man7.org/linux/man-pages/man1/nsenter.1.html
* https://www.redhat.com/sysadmin/container-namespaces-nsenter
