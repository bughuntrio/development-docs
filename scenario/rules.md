---
layout: subpage
---

## Rules and requirements

Whilst we work to provide the most flexible environment possible, we place the following rules and limitation on scenarios which are hosted on [bughuntr.io](https://bughuntr.io):

* Scenarios should not require excessive brute force, fuzzing or automation.
  * Users are rate-limited to 20 requests per second from a single IP address
  * As such, we recommend any fuzzing can be completed with standard wordlists (*In future we may recommend specific wordlists*)

* All web services should implement a `_ping` endpoint returning a 200 response with the `Access-Control-Allow-Origin: *` HTTP header set. If this is not possible, set the `display.forceload` metadata value to `true`, e.g.
```yaml
display:
    forceload: true
```

* Port scanning of the external environment is currently not permitted as external IPs are shared across multiple running scenarios. Scenarios should specify external services in the scenario description, or have them readily discoverable via scenario functionality.

* Unless *absolutely* required, scenarios should be designed to work in 512MiB or less of RAM.
