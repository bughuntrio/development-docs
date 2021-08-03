---
layout: subpage
---

## Metadata - Conf expose

External network services are exposed from a scenario via the `conf.expose` configuration.

The expose section is used for exposing ports from the scenario, it expects an array of objects with the following keys:
* `port` *int* The port number exposed from the scenario
* `direct` *bool* Whether the port should be directly accessible, or accessible through a reverse proxy on standard web ports **Default:** false
* `domain` *string* The subdomain name to use when proxying the exposed port **Default:** None

### Port mapping
Ports specified in the `expose` configuration should map to ports exposed from the scenario.

In most cases, when exposing HTTP services the `direct` parameter should be left unset. In this default case, the HTTP service will be accessible via the standard web ports (80/tcp and 443/tcp) routed though a reverse proxy.

Where the `direct` parameter is set, the port numbers specified will not necessarily map to the ports the user will connect over. As multiple scenarios execute on a host at once, port mapping is performed to ensure there is no clashes of port numbers. For example, with an exposed port of `8001/tcp` from the scenario, the user may end up connecting to the service over the external port `31029/tcp`.

### TLS certificates
Where HTTP services are access via the reverse proxy (e.g. when `direct` is `false`), valid TLS certificates for the scenario domain will be provided from the reverse proxy.

### UDP services
UDP services are not currently supported. However, this feature is in our development plan. If you require this feature for your scenario please contact us via [support@bughuntr.io](mailto:support@bughuntr.io).

### Examples

#### HTTP service
The following configuration will allow a scenario HTTP service to be accessed via HTTP/S on the standard web ports (80/tcp and 443/tcp) via the reverse proxy.
```yaml
conf:
  expose:
  - port: 80
```

#### TCP service
The following configuration will directly expose a TCP service from the scenario, bypassing the reverse HTTP proxy
```yaml
conf:
  expose:
  - port: 9001
    direct: true
```

#### Adding an extra subdomain
The following configuration will add a subdomain `dev` which resolves to the IP address of the scenario, but without adding any additional exposed services.
```yaml
conf:
  expose:
  - port: 80
  - domain: dev
```
