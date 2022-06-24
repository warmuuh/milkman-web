
---
title: "Milkman Cassandra Plugin"
linkTitle: "Cassandra"
description: "Introduces Cql Requests to Milkman using cassandra datastax driver."
resources:
- src: "**.{png,jpg}"
  title: "Image #:counter"
---

{{% pageinfo %}}
Introduces Cql Requests to Milkman using cassandra datastax driver.
{{% /pageinfo %}}

# Requirements
This plugin requires `milkman-jdbc` plugin

# Installation
After placing the jar into `\plugin` folder, you also have to place the driver-jars you want to use in that folder as well.

# Usage

The used url is of following format:
```
cql://host[/keyspace]?dc=...[&username=...&password=...]
```

supported parameters

| name | description |
| --- | --- |
| dc | datacenter, required. for local installations, this should be 'datacenter1' |
| username | username, optional |
| password | password, optional |

# Screenshot

{{< imgproc cassandra Resize "x500" >}}
Editing a secret oAuth Key (Password Grant).
{{< /imgproc >}}

# Features

 * Execution of Requests against cassandra databases
