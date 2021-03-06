---
alias: aip7oojeiv
path: /docs/reference/simple-api/subscriptions
layout: REFERENCE
shorttitle: Subscriptions
description: Use subscriptions to receive data updates in realtime. Subscriptions in the GraphQL schema are derived from types and relations.
simple_relay_twin: eih4eew7re
tags:
  - simple-api
  - subscriptions
related:
  further:
    - bag3ouh2ii
    - oe8oqu8eis
    - ohmeta3pi4
    - kengor9ei3
---

# Subscriptions in the Simple API

*GraphQL subscriptions* allow you to be notified in realtime of changes to your data.
All available subscriptions are automatically generated. To explore them, use the [playground](!alias-oe1ier4iej) inside your project.

You subscribe to a subscription using your [endpoint](!alias-yahph3foch#project-endpoints) using websockets.

This is an example subscription that notifies you whenever a new post is created:

```graphql
---
endpoint: https://api.graph.cool/simple/v1/cj03vcl4777hv0180zqjk5a6q
disabled: true
---
subscription newPosts {
  Post(
    filter: {
      mutation_in: [CREATED]
    }
  ) {
    mutation
    node {
      description
      imageUrl
    }
  }
}
---
{
  "data": {
    "Post": {
      "mutation": "CREATED",
      "node": {
        "description": "#bridge",
        "imageUrl": "https://images.unsplash.com/photo-1420768255295-e871cbf6eb81"
      }
    }
  }
}
```

You can subscribe to a single mutation or combine multiple mutations that you want to subscribe on. In both cases you can use filters to describe exactly what mutation events you are interested in.
