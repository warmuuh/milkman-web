
---
title: "Advanced Features"
linkTitle: "Features"
description: "Description of some more advanced Features of Milkman"
resources:
- src: "**.{png,jpg,gif}"
  title: "Image #:counter"
---

{{% pageinfo %}}
Description of some more advanced Features of Milkman
{{% /pageinfo %}}


# Code Folding

* Folding is supported in response body area
* Toolbar actions: expand all, collapse all, expand one level, collapse one level
* Clicking on the line-symbol expands the node
* Right-Clicking on the line-symbol expands the whole subtree

![Example of code folding](/images/folding.gif)

# Hotkeys

  * <kbd>CTRL</kbd>+<kbd>ENTER</kbd> - Execute Request
  * <kbd>CTRL</kbd>+<kbd>N</kbd> - New Request
  * <kbd>CTRL</kbd>+<kbd>R</kbd> - Rename Active Request
  * <kbd>CTRL</kbd>+<kbd>W</kbd> - Close Active Request
  * <kbd>CTRL</kbd>+<kbd>S</kbd> - Save Active Request
  * <kbd>CTRL</kbd>+<kbd>E</kbd> - Edit current Environment
  * <kbd>CTRL</kbd>+<kbd>Space</kbd> - Quick-Edit of Variables
  * <kbd>ESC</kbd> - Cancel running Request
  

# Copy&Paste in Tables

* You can <kbd>CTRL</kbd>+<kbd>C</kbd> selected rows to copy its value
* You can <kbd>CTRL</kbd>+<kbd>V</kbd> multiple rows into a table


![Example of Copy&Paste for tables](/images/copypaste.gif)

# Quick Edit for Variables

* You can highlight variables.
* Clicking on it opens popup for modification/creation
* <kbd>ESC</kbd> hides highlighting

![Example of Quick-edit for variables](/images/highlight-vars.gif)

# Keys

* secret keys that should not be exported or synced can be setup using the key-symbol
* currently, secret keys are only plain type keys, but will be e.g. oauth-keys etc
* can be accessed using {{key:name-of-key}} variable


# Libraries

you can register libraries to easily look-up and import services from a central registry, such as [APIs.guru](http://apis.guru).

![Example of setting up and using Libraries](/images/milkman-library.gif)
