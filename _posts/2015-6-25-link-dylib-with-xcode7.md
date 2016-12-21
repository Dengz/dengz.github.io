---
layout: post
title: How to link dylibs in Xcode7
---

* Delete all references to .tbd files from either your linked libraries phase, or from the copied bundle resources phase (where they sometimes will be added).
* Add the library you want to link manually to the "Other Linker Flags" build settings, by adding the argument: -l for each library you want to link (for example, add "-lsqlite3" (without quotes)).
