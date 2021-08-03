---
layout: subpage
---

## Scenario images

Custom Open Container Initiative (OCI) images (commonly Docker images) can be executed as part of a scenario. Custom images should be provided as source, with a build script (or `makefile`) to compile any required source code and `Dockerfile` to create the container image. A `scenario.yaml` file must be included which specifies the [metadata](/scenario/meta.html) for the scenario.

A basic image file listing might look like:
```
.
├── assets
│   └── index.html
├── build.sh
├── Dockerfile
├── scenario.yaml
└── src
    └── nginx.conf
```

See the [Simple](/examples/simple.html), [Complex](/examples/complex.html) and [Virtualised](/examples/virtualised.html) scenarios for example implementations.
