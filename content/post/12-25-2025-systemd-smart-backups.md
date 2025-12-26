---
title: Systemd Smart Backups
author: LQR471814
date: 2025-12-25
categories:
- tutorials
tags:
- programming
- linux
---

It is often desirable to make backups of a server's data to some
remote location, however said backups often take up lots of CPU
and IO time.

Even with a tool like BorgBackup which is quite efficient at
diffing versions and encrypting on the fly, backups can still
stall the system if a significant number of changes have built-up
since the last backup time.

As such, it would be more sensible to back up more frequently and
reduce the likelihood of building up large diffs. And ideally,
these frequent backups would be "nice" to other processes and let
more important processes take priority before continuing execution.

We can do this quite easily with systemd.

Let's create the service that runs the backup.

```ini
[Unit]
Description=Backup Service

[Service]
Type=simple
# only give CPU cycles to this process if no other processes are running
CPUSchedulingPolicy=idle
# only give IO time to this process if no other processes are running
IOSchedulingClass=idle

# run backup
ExecStart=/usr/bin/borg create /path/to/repo::arch /path/to/data
```

Let's create a timer unit that runs this service periodically.

```ini
[Unit]
Description=Run Backup every 6 hours

[Timer]
# Run 15 minutes after the system boots up
OnBootSec=15min
# Run every 6 hours thereafter
OnUnitActiveSec=6h
# Ensure the job runs at the next opportunity if the server was off
Persistent=true

[Install]
WantedBy=timers.target
```

Now enable the timer with `sudo systemctl enable
some-backup.timer` and systemd will smartly handle cases like:
"new run happens while backup is still running, waiting for a
required service (like network), or etc..."
