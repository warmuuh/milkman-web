
---
title: "oAuth support arrived"
linkTitle: "oAuth support"
date: 2021-04-27
description: >
  Milkman has now native support for all kinds of authentication, most importantly though oAuth
---

# oAuth support shipped

Milkman now supports oAuth2 keys and more general provides a (naturally extensible) way to organise secret credentials.

# Keys

Putting credentials into environment variables works but had its downsides. When syncing your workspace you might not want to store secrets in e.g. github. Therefore the concept of keys for introduced. 

Those keys live next to an environment and are not synched or exported, so they won't leave your milkman.

Keys are basically objects that have a string value, but it is up to the plug-in to come up with that value. A simple example is a base64 value of a given string.

# oAuth support

Another example is oAuth2 support. The actual value of the key will be retrieved and cached from an authorization server. Refresh of a token will happen implicitly, so once you've setup your oAuth data, you can use that key without thinking about it again.

# other types of keys

I have no plans yet to add support for other kind of keys but some ideas. I could think of supporting general jwt minting or integration with an external credential storage such as vault, let's see...