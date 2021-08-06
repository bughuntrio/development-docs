---
layout: subpage
---

## Metadata - Executors

Executors are responsible for executing scenario [commands](/scenario/commands.html). There are currently two executors `docker` and `qemu`, each with their own configuration.

The choice of executor should be specified in the `executor.type` key, for example to select the `docker` executor use:
```yaml
executor:
  type: docker
```

### Docker executor

The docker executor executes OCI container images via Docker, this executor should be sufficient for 90% of use cases. It is fast to set up and can accommodate complex multiple container scenarios easily.

For security reasons, the Docker executor has certain limitations. Containers cannot be executed in a way that could lead to compromise of the executor host. So privileged containers are not permitted, and certain limitations are placed on container configurations (see [commands](/scenario/commands.html) for further details).

#### Configuration

Executor configuration should be specified under the `executor.conf` key, with the following options:

* `ram` *int* Default ram limit for created containers
  * The total RAM used by the scenario should not exceed 512MiB
* `flags` *dict -> string[]* Map of container names to an array of flag names, used for mounting flags into named containers automatically.

The `flags` configuration is required to specify which container to map a named flag into. For example, to place a flag called `Root` into a container called `webserver` at `/root_flag` the following configuration should be used:

```yaml
executor:
  type: docker
  conf:
    flags:
      webserver:
        - Root
conf:
  flags:
    - type: file
      name: Root
      path: /root_flag
```

For more details on flags see [Conf flags](/scenario/flags.html).

### QEMU executor

 The QEMU executor runs scenario commands in the virtual machine, using a specified container image as the root filesystem. The QEMU executor should be used when the privileges provided by the Docker executor are not sufficient, or when a more exotic scenario is planned.

#### Configuration

Executor configuration should be specified under the `executor.conf` key, with the following options:

* `ram` *int* Default ram limit for created containers
  * The total RAM used by the scenario should not exceed 512MiB
* `image` *string* The container repository URL of an image to use as the root filesystem of the virtual machine
* `args` *string[]* The command to run as PID 1 in the image. If supplied overwrites the image defined  `Entrypoint` and `Cmd`
* `env` *string[]* Environment variables to pass into the image. If supplied overwrites the image defined `ENV`

**Note:** The entrypoint and cmd configured in the specified image will execute as PID 1 when the virtual machine starts up.

For example, to use the BusyBox container image from docker.io as the root filesystem for a QEMU scenario use:
```yaml
executor:
  type: qemu
  conf:
    ram: 256
    image: docker.io/library/busybox:latest
```

Unlike the docker executor, there is no automatic handling of flags in the QEMU executor. Flags will simply be copied into the root filesystem of the virtual machine.
