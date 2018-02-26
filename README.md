# zfs-auto-backup

`zfs-auto-backup` automatically backs up your local ZFS snapshots to another pool via
`zfs send | zfs recv`. This tool is recommended to be used together with `zfs-auto-snapshot`,
which can be found [here](https://github.com/zfsonlinux/zfs-auto-snapshot).

Designed for ease of configuration, this tool reads data from zfs properties documented below.

```bash
usage() {
    echo -n "
Usage: $0            : run backup on all datasets configured for zfs-auto-backup
       $0 help       : display this message

zfs-auto-backup automatically transfers local zfs snapshots that match the given criteria
to the configured alternative pool (via zfs send | zfs recv), set via \"org.jsteward:auto-backup*\"
zfs property keys. The following keys are used:

    # marks whether snapshots of this dataset are candidates for backup to external disk
    org.jsteward:auto-backup = ( false | true )
    # marks the destination to send the snapshots to (parent of the backups that will be stored in)
    org.jsteward:auto-backup-dest = [ parent path ]
    # marks the tag of the snapshots to send
    org.jsteward:auto-backup-tag = [ tag name ]

Additionally, \"org.jsteward:auto-backup-origin\" will be set for the destination dataset, denoting
the full name which the backup came from, which may help when restoring the backup.

"
    exit 127
}
```

`zfs-auto-backup` has some variables available for overriding upon calling:

 - `DEBUG` controls whether debug messages will be printed or not. (default: `1`)
 - `DRY_RUN` controls whether real actions will be performed or simply printing out what will be done. (default: `0`)
 - `PREFIX` specifies prefixes in snapshot naming scheme (default: `znap` , as in `@znap_2018-01-01-0000_hourly`)
 - `OVERRIDE_TARGET` specifies whether the destination should be overrided when conflicts while receiving occurs

They can be set in your environment, or overrided on the commandline like this:

```bash
DRY_RUN=1 ./zfs-auto-backup
```

This tool is a bash script tested on `GNU bash, version 4.4.19(1)-release (x86_64-unknown-linux-gnu)`.
More tests on other shells are welcomed.
