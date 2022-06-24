
---
title: "Milkman Auth Plugin"
linkTitle: "Authentication"
description: "Contains key-types for authentication, such as oauth-authentication"
resources:
- src: "**.{png,jpg}"
  title: "Image #:counter"
---

{{% pageinfo %}}
Contains key-types for authentication, such as oauth-authentication
{{% /pageinfo %}}


# Features
* supports oauth2
  * password-grant, client-credential grant and authorization-code grant

# Screenshot

{{< imgproc auth Resize "x500" >}}
Editing a secret oAuth Key (Password Grant).
{{< /imgproc >}}


# Remarks
* does not yet support specifying a redirect-url for authorization-code as there is currently no browser included.

