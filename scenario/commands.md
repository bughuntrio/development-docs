---
layout: subpage
---

## Metadata - Conf commands

Scenario commands are run by executors to set up a scenario environment for the user. The `conf.commands` key should specify an array of commands to run to set up the environment. Commands are executed synchronously, one after the other.

Commands should specify the following keys:
* `type` *string* Currently there is a single command type `docker`
* `connect` *bool* Whether to connect a shell to the command input/output
  * When the `connect` value is specified, as per the synchronous execution of commands, the next command will only run once the connected command terminates
* `spec` *object* The command specification, see below

For example to select a `docker` command use:
```yaml
conf:
  commands:
    - type: docker
      spec:
        ...
```

**Note:** At this time, all scenarios with a `qemu` executor which require the use of commands should run a docker daemon bound to `0.0.0.0:2385/tcp` with no TLS requirement.

### Docker commands

#### Run

Run a new container. Run commands can specify the following options in the `conf.commands.spec.run` key:
* `image` *string* The container repository URL of an image to run
* `name` *string* The name of the container
* `ports` *string[]* An array of `<host port>:<container port>` mapping pairs to map a container port to a host port
  * See [Conf expose](/scenario/expose.html) for further details
* `user` *string* The username of the user to run as
* `entrypoint` *string[]* The entrypoint of the container **Default:** The entrypoint specified by the image
* `cmd` *string[]* The command to run in the container entrypoint **Default:** The command specified by the image
* `env` *string[]* The environment of the container **Default:** The environment specified by the image
* `working-dir` *string* The working directory of the container **Default:** The working directory specified by the image
* `tty` *bool* Whether to expose a TTY to the container
* `read-only` *bool* Whether the container file system should be read-only
* `security-opt` *string[]* Security options for the container
  * The only permitted `security-opt` when using the `docker` executor is `no-new-privileges` which prevents a user from gaining elevated privileges within the container
* `cap-drop` *string[]* Capabilities to drop for execution of the container
* `read-only-paths` *string[]* Read only paths (see the docker run documentation)
* `masked-paths` *string[]* Masked paths (see the docker run documentation)

The following options are only available when running with the `qemu` executor:
* `privileged` *bool* Whether the container should run in privileged mode
* `cap-add` *string[]* Capabilities to add for execution of the container
* `security-opt` *string[]* Additional security options for the execution of the container
* `network` *string* Which network to connect the container to
* `mounts` *object[]* File and directory bind mounts to mount into the container
  * Should specify a `source` key and a `target` key, and an optional `read-only` key

These options map directly to the options of the `docker run` command, for further details see the [`docker run` documentation](https://docs.docker.com/engine/reference/commandline/run/)

##### Examples
To run a container using the busybox image and connect a shell to it, use:
```yaml
conf:
  commands:
    - type: docker
      connect: true
      spec:
        run:
          image: docker.io/library/busybox:latest
```

To run a container using the nginx image exposing port 80, use:
```yaml
conf:
  commands:
    - type: docker
      spec:
        run:
          image: docker.io/library/nginx:alpine
          ports:
            - 80:80
```

To mount a file from the host filesystem into a container on a scenario with a `qemu` executor use:
```yaml
executor:
  type: qemu
  conf:
    ram: 256
    image: docker.io/library/docker:dind
    args:
      - dockerd
      - --tls=false
      - --host=0.0.0.0:2375
conf:
  commands:
    - type: docker
      spec:
        run:
          image: docker.io/library/alpine:latest
          mounts:
            - source: /flag1
              target: /flag
```

#### Exec

Execute a command in an already running container. Exec commands should specify the following options in the `conf.commands.spec.exec` key:
* `name` *string* The name of the container in which to execute the command
* `user` *string* The username of the user to run as
* `working-dir` *string* The working directory of the container **Default:** The working directory specified by the image
* `cmd` *string* The command to execute
  * The command is executed within the `/bin/sh` shell

For example, to execute the `/bin/id` command in a previously started container called `main` use:
```yaml
conf:
  commands:
    - type: docker
      spec:
        run:
          image: docker.io/library/busybox:latest
          name: main
    - type: docker
      spec:
        exec:
          name: main
          cmd: /bin/id
```
