---
layout: subpage
---

## Metadata - Display

Display affects all things display like in the scenario start page. It has the following options
* `shell` *bool* Whether or not the scenario should display and connect to a shell
* `iframe` *string* An initial service URL to display in an iframe as a starting point for the scenario.
  * If hostname of the URL ends with `scenaro.bughuntr.net` it will be replaced with the assigned hostname of the scenario, e.g. `https://scenario.bughuntr.net/` will be rewritten to `https://fluffybunny.br1.bughuntr.net`
  * The URL port will be replaced with the mapped port of the services
* `services` *string[]* An array of service URLs to be listed on the scenario page
  * If hostname of the URL ends with `scenaro.bughuntr.net` it will be replaced with the assigned hostname of the scenario, e.g. `https://scenario.bughuntr.net/` will be rewritten to `https://fluffybunny.br1.bughuntr.net`
  * The URL port will be replaced with the mapped port of the services
  * The `tcp` scheme should be used for non-http services.
  * Services with an `http` or `https` scheme should implement a `_ping` endpoint which returns a `200 OK` response with `Access-Control-Allow-Origin: *` indicating that the service is ready to use
* `forceload` *bool* Whether the service URLs should be displayed without checking for a `/_ping` endpoint
* `horizontal` *bool* Whether the emdedded shell or iframe should be positioned with the description above, or the description by the side.
