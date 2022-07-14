
---
title: "Milkman Grpc Plugin"
linkTitle: "Grpc"
description: "Grpc Plugin for communication with Grpc Servers."
resources:
- src: "**.{png,gif}"
  title: "Image #:counter"
---

{{% pageinfo %}}
Grpc Plugin for communication with Grpc Servers.
{{% /pageinfo %}}

## Features

  * Can work with [Server Reflection](https://github.com/grpc/grpc/blob/master/doc/server-reflection.md) to query services and retrieve *.proto files
  * Given a *.proto file, Server Reflection is not necessary to query a service
  * Read/write ASCII headers
  * Support Server/client/both streams
  
  
## Screenshot

{{< imgproc plugin Resize "x500" >}}
Example of the Grpc plugin
{{< /imgproc >}}

### Example of Server streaming

![Example of Grpc Streaming](/images/grpc-streaming.gif)


### Client Streaming

To send multiple messages, just add multiple json objects to the payload, divided by two new lines.
