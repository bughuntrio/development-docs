---
layout: subpage
---

## Tips

* Where brute-forcing is required use either Seclists `common.txt` or expose the desired result through osint
  * There are rate limits in place on the reverse proxy to prevent excessive load on the shared infrastructure
* In general, services should run as a low privileged user, this will help reduce the number of unintended 'solutions' to your scenario.
* Where a database is required, consider using a lightweight file based database such as SQLite3. This will reduce the complexity and memory footprint of the scenario.
* Port scanning of 'external' is not currently supported, but this functionality is actively being worked on
  * This is due to the shared nature of the infrastructure
* Scenarios which * absolutely require* unencrypted HTTP (i.e. not HTTPS) cannot be iframed due to mixed-content restrictions
