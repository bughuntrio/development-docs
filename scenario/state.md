---
layout: subpage
---

## Scenario runtime state

Not all users will choose to, or be able to, complete a scenario in a single session. Where a scenario requires significant user content creation or configuration (for example writing and submitting source code), state should be used to save user progress across runs.

The runtime API URL base can be accessed via the `RUNTIME_API_URL` environment variable, and authentication performed via token in the `RUNTIME_API_TOKEN` environment variable. To access the state API, the `/api/v1/runtime/state` endpoint should be used.

The state API has the following endpoints:
* [`GET`, `PUT`, `DELETE`] `/api/v1/runtime/state/<key>` - Store / retrieve data in the key value store.
  * The data will be served with the `Content-Type` that the value was `PUT` with.
  * `GET`ting from this endpoint will return the data directly.
  * Only text should be stored in this store, an attempt to store binary data will respond with a 500 error.
  * This store has a hard limit of 32KiB of data per key, if you require more space than this use the `document` store, attempting to add more data than this will result in a 400 response.
* [`GET`, `PUT`, `DELETE`] `/api/v1/runtime/state/document/<key>` - Store / retrieve data in the `document` store.
  * The data will be served with the `Content-Type` that the value was `PUT` with.
  * `GET`ting from this endpoint will respond with a 302 redirect to the S3 bucket location of the stored document.
  * Any kind of data can be stored in this store.

As a guide, if you are storing more than a few lines of data, or storing binary data, then the `document` store should be used.

### Examples
To get the value of the `data` key from the state store, use:
```sh
curl -H "Authorization: Bearer ${SCENARIO_API_TOKEN}" "${SCENARIO_API_URL}/api/v1/runtime/state/data"
```

To store a value 'testdata' in the state store against the `data` key use:
```sh
curl -X PUT --data "testdata" -H "Authorization: Bearer ${SCENARIO_API_TOKEN}" "${SCENARIO_API_URL}/api/v1/runtime/state/data"
```

To store the contents of the file `/tmp/file` in the state store against the key `/tmp/file` use:
```sh
curl -X PUT --data-binary "@/tmp/file" -H "Authorization: Bearer ${SCENARIO_API_TOKEN}" "${SCENARIO_API_URL}/api/v1/runtime/state/document/tmp/file"
```
