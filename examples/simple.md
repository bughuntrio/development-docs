---
layout: subpage
---

## Simple scenario example

[Repository](https://github.com/bughuntrio/example_scenarios/tree/main/simple)

This scenario runs a single Docker container, exposing a web port, with a single flag accessible at `/root` within the container.

```yaml
name: Simple Scenario
category: Examples
executor:
  type: docker
  conf:
    ram: 256
    flags:
      webserver:
        - Root
conf:
  # Expose port 8000/tcp via the reverse web proxy
  expose:
    - port: 8000
  commands:
    - type: docker
      spec:
        run:
          image: docker.io/bughuntrio/examples-simple:latest
          name: webserver
          # Map port 8000/tcp from the container to port 8000/tcp on the host
          ports:
            - 8000:8000
  flags:
    - type: file
      name: Root
      path: /flag
      perms: 0o600
display:
  iframe: https://scenario.bughuntr.net/
  # As the HTTP server does not implement the `/_ping` endpoint, use forceload
  forceload: true
```
