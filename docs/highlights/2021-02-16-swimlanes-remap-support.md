---
last_modified_on: "2021-02-16"
$schema: ".schema.json"
title: "Remap support for the `swimlanes` transform (routing)"
description: "Use VRL to specify conditions for routing events into multiple channels"
author_github: "https://github.com/lucperkins"
pr_numbers: []
release: "0.12.0"
hide_on_release_notes: false
tags: ["type: enhancement", "transform: swimlanes"]
---

The [`swimlanes`][swimlanes] transform for Vector enables you to route events into
multiple named channels, or *lanes*, based on supplied conditions. This can be
useful for use cases like sending a subset of events to one sink and a different
subset to another.

Previously, the `swimlanes` transform required you to specify conditions using
`check_fields`. But now you can use Vector Remap Language expressions in the
[`remap`][remap] transform to specify those conditions. This should make using
`swimlanes` more natural and expressive.

## Example

The example configuration below shows the same `swimlanes` transform using the
old system (`check_fields`) and the new system (`remap`):

```dff
 [transforms.split_events]
 type = "swimlanes"
 inputs = ["http-server-logs"]

 # Using check_fields
-lanes.success.type = "check_fields"
-lanes.success.status_code.eq = 200
-lanes.success.severity.eq = "info"

 # Using remap
+lanes.success.type = "remap"
+lanes.success.source = '.status_code == 200 && .severity == "info"'
```

[swimlanes]: https://vector.dev/docs/reference/transforms/swimlanes
[remap]: https://vector.dev/docs/reference/transforms/remap
