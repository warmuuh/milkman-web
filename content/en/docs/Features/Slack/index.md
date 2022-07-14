
---
title: "Slack Integration"
linkTitle: "Slack"
description: "Slack-integration for milkman"
resources:
- src: "**.{png,jpg}"
  title: "Image #:counter"
---

{{% pageinfo %}}
Milkman supports slack-integration via a [slackbot](https://github.com/warmuuh/milkman-slack).

{{% /pageinfo %}}

You can either host it yourself or use the heroku-hosted instance:

[![Add to Slack](https://platform.slack-edge.com/img/add_to_slack.png)](https://milkman-slack.herokuapp.com/slack/oauth/start)

# Usage

Once added to your workspace, you can share a private-bin export url via:

```
/milkman <privatebin-url>
```

# Features

It will nicely render the shared request and offers several ways for viewers to use the request, 
such as viewing the request as `curl`-command or `http`-request.

{{< imgproc preview Resize "x500" >}}
Preview of a request in slack
{{< /imgproc >}}


and users can choose how to view the request:

{{< imgproc slack-render Resize "x500" >}}
Quick-export of a request directly in slack.
{{< /imgproc >}}


it also supports all other request types, such as gRPC:

{{< imgproc grpc Resize "x500" >}}
Example of a Grpc request in slack
{{< /imgproc >}}


# Planned

More features are planned, such as:

* execution of requests on backend
* support for burn-after-reading in privatebin
* more templates