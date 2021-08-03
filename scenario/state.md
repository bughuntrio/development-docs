---
layout: subpage
---

## Scenario state

**Please Note:** The scenario state API is currently undergoing an overhaul. We hope to have it finalised by the end of August 2021.

Not all users will choose to, or be able to, complete a scenario in a single session. Where a scenario requires significant user content creation or configuration (for example writing and submitting source code), state should be used to save user progress across runs.

The scenario API URL base can be accessed via the `SCENARIO_API_URL` environment variable, and authentication performed via token in the `SCENARIO_API_TOKEN` environment variable. To access the state API, the `/state` endpoint should be used. For example, to store the contents of a file in the scenario state use:
```sh
cat file | curl -X PUT --data-binary @- -H "Authorization: Bearer ${SCENARIO_API_TOKEN}" "${SCENARIO_API_URL}/state"
```

The state can similarly be retrieved via:
```sh
curl -H "Authorization: Bearer ${SCENARIO_API_TOKEN}" "${SCENARIO_API_URL}/state"
```
