# fstab

This Role generates a proper nice fstab.


## Requirements

A running linux system.


## Role Variables

| Name                           | Type                                | Default/Required [¹](#__required)   | Description                                                                                                                                      |
|--------------------------------|-------------------------------------|:-----------------------------------:|--------------------------------------------------------------------------------------------------------------------------------------------------|
| `fstab_auto_root`              | boolean                             | `true`                              | Auto generate a fstab entry for the root partion (based on current mount).                                                                       |
| `fstab_auto_root_check`        | boolean                             | `true`                              | Set the fsck flag for auto generated root entry                                                                                                  |
| `fstab_auto_root_dump`         | boolean                             | `true`                              | Set the dump/backup flag for the auto generated root entry                                                                                       |
| `fstab_auto_root_options`      | list of strings                     | ___obtained from mount___           | Overwrite the current mount options                                                                                                              |
| `fstab_auto_root_type`         | string                              | ___obtained from mount___           | Overwrites root mount type (useful for network filesystems, for ex. if you change a cifs to a nfs based backend)                                 |
| `fstab_tmpfs`                  | boolean                             | `true`                              | Generate a fstab entry for /tmp on a tmpfs                                                                                                       |
| `fstab_tmpfs_max_size`         | integer                             | **50% by default(tmpfs default)**   | Maximum size of the tmpfs                                                                                                                        |
| `fstab_tmpfs_options`          | list of strings                     | `[ 'nodev', 'nosuid' ]`             | mount options for the tmpfs                                                                                                                      |
| `fstab_sysfs`                  | boolean                             | `true`                              | Generate a fstab entry for sysfs                                                                                                                 |
| `fstab_sysfs_options`          | list of strings                     | `['defaults']`                      | sysfs mount options                                                                                                                              |
| `fstab_proc`                   | boolean                             | `true`                              | Generate a fstab entry for procfs                                                                                                                |
| `fstab_proc_options`           | list of strings                     | `['defaults']`                      | procfs mount options                                                                                                                             |
| `fstab_additional_mounts`      | [list of dicts](#additional-mounts) | `[]`                                | Add additional mount entries                                                                                                                     |
| `fstab_reload`                 | boolean                             | `false`                             | Reload config after changes                                                                                                                      |
| `fstab_reload_method`          | string                              | `mount`                             | Reload method for changes in fstab, currently only `mount` is implemented, which executes `mount -a`. `reboot` may be implemented in the future. |
| `fstab_create_additional_dirs` | boolean                             | `false`                             | Create the `target` path for each entry in `fstab_additional_mounts`                                                                             |

<a id="__required">¹</a> Variable is not required unless no default is given or other specified


### Additional mounts
| Name               | Type            |          Default/Required [¹](#__required)           | Description                                                                                                                |
|--------------------|-----------------|:----------------------------------------------------:|----------------------------------------------------------------------------------------------------------------------------|
| `source`           | string          |                                                      | Source device/filesystem to mount                                                                                          |
| `target`           | string          |                                                      | Path to mount on (mount point)                                                                                             |
| `type`             | string          |                                                      | Type of the filesystem                                                                                                     |
| `options`          | list of strings | `['defaults']` ___at least one option is required___ | Mount options to set                                                                                                       |
| `dump`             | boolean         |                        `false`                       | Set the dump/backup flag                                                                                                   |
| `fsck`             | integer         |                          `2`                         | Set the fsck flag (keep in mind that a value of `1` is only recommended for the root fs and a value of `2` for all others) |

For additional information see `man fstab`.


## Example Configuration

```yml
fstab_swapfile: 4G
fstab_auto_root_options:
  - remount
  - rw
  - _netdev
fstab_additional_mounts:
  - source: /dev/sdb3
    target: /mnt/data
    type: ext4
    options:
      - ro
      - nodev
    dump: true
```

## License

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](https://creativecommons.org/licenses/by-sa/4.0/).


## Author Information

- [Markus Mroch (Mr. Pi)](https://github.com/Mr-Pi) _markus.mroch@stuvus.uni-stuttgart.de_
