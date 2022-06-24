
---
title: "Milkman Http Plugin"
linkTitle: "Http/Rest"
description: "Allows to share requests etc via Privatebin."
resources:
- src: "**.{png,gif}"
  title: "Image #:counter"
---

{{% pageinfo %}}
Introduces Http request capabilities to milkman. Consists of serveral features that together should make milkman be usable as postman-replacement for day-to-day work.
{{% /pageinfo %}}

# Screenshot

{{< imgproc screenshot Resize "x500" >}}
Example of the Rest plugin
{{< /imgproc >}}

### Example of Server Sent Events Streaming

{{< imgproc sse Resize "x500" >}}
Example of Server-sent event streaming in the Rest plugin
{{< /imgproc >}}


# Features

 * Postman-like UI
 * Crafting of requests by editing body, headers, parameters
 * Highlighting/formatting for json
 * Proxy-Authentication support (BASIC for now)
 * Importers for Postman exports (Collections, Environments, Data-Dump)
 * Importers for OpenApi v3.0
 * (planned) Exporters
 * Support import of APIs listed at [APIs.guru](https://apis.guru/), see [demo](/img/gif/milkman-library.gif)


