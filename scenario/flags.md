---
layout: subpage
---

## Metadata - Conf flags

Generally, flags for scenarios are generated on a per user, per scenario, per flag basis. This means that no two users will be given the same flag for the same scenario. Flags are specified in the scenario metadata Yaml `conf.flags` section.

There are currently two types of flags, `file` flags which have their flag values copied into the scenario filesystem at runtime, and `static` flags which use a single static value for all users.

## File flags

`file` flags should specify the following properties:
* `name` *string* The name of the flag (try to avoid using the generic name 'Flag') **Note:** The flag `name` will be displayed to the user when they successfully submit the flag
* `path` *string* The path the flag should be placed at runtime
* `perms` *octal* The octal file permission of the flag, e.g. `0o600` **Default:** 0
* `uid` *string* The user id for file ownership **Default:** 0
* `gid` *string* The group id for file ownership **Default:** 0

`file` flags should be the primary type of flag used.

```yaml
conf:
  flags:
    - type: file
      name: Root
      path: /flag
      perms: 0o600
```

## Static flags

`static` flags should be used when flags are hidden outside of the scenario file system, for example in a public GitHub repository. They should be used only when *strictly* necessary.

`static` flags should specify the following properties:
* `name` *string* The name of the flag (try to avoid using the generic name 'Flag') **Note:** The flag `name` will be displayed to the user when they successfully submit the flag
* `value` *string* The value of the flag, this should be of the format `$flag{flag_value_here}`

```yaml
conf:
  flags:
    - type: static
      name: Repository hidden
      value: $flag{this_is_the_flag}
```
