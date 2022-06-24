
---
title: "Milkman Git Sync Plugin"
linkTitle: "Team Synchronization via Git"
description: "Team Synchronization via Git"
resources:
- src: "**.{png,gif}"
  title: "Image #:counter"
---

{{% pageinfo %}}
Support for Socket.IO transport
{{% /pageinfo %}}

This plugin allows to setup a git repository to where a workspace can be synchronized. Synchronization works by computing the diffs and applying them in a fuzzy manner to the latest version (simple algorithm of Differential Synchronization).

# Screenshot

{{< imgproc plugin Resize "x500" >}}
Example of the Scripting plugin
{{< /imgproc >}}

# Features
 * synchronizes (on-demand for now) workspaces with a remote git repository

# Remark
 * This feature is experimental.