---
title: "Fast Startup in Milkman using AppCds"
linkTitle: "Fast Startup in Milkman"
date: 2019-07-26
description: >
  Description on several techniques used in milkman to shave of the time till usable
---

## td;dr;
Milkman dynamically (re-)generated AppCds cache to improve startup time of a JavaFx application.This blog post goes into how to apply AppCds on a desktop application with real-world requirements like shipping, updates and plugins.

# Milkman
Currently, I am developing Milkman, an alternative to Postman that is extensible and faster (as it is not using electron, but *sigh* the more lightweight JavaFx stack.

One goal of this application is to be a "jump-in and do something, then leave" type of application. I don't want to wait 10 seconds until it is ready to be used before doing a small request and closing it again.

Although Milkman starts already quite fast (compared to e.g. electron apps), it can be optimized further. One option is to use GraalVm to compile to native. Although this would be possible (due to usage of a compile-time dependency injection framework, hardwire), the project is not yet in a stage where it can be setup easily, not to mention windows support. Also JavaFx is on its way to supporting GraalVm.

# Application Class Data Sharing (AppCds)

Another option, that might be lesser known, is to use Class Data Sharing (CDS) or more specifically AppCds (which extends CDS on application classes).  AppCds basically caches all information about loaded classes in a file that can be mapped to memory instead of rescanning the whole classpath on every start.
This was a feature in OracleJdk but is available in OpenJdk as well starting with jdk 10. As Milkman comes with a custom JRE, I can rely on having the right JDK for profiting from AppCds.

There is a pretty good article about AppCds at codeFx. They do mention that it is hard to apply AppCds on JavaFx applications as javafx is not packaged anymore with the JDKs. For that reason, I moved to use Liberica as a JDK, which is basically openJdk + JavaFx (and some other features, I guess). The mentioned blog already contains pretty good details on how to initialize AppCds, so I wont repeat those details here. The final result will be, in the case of Milkman, a ~80Mb file. The startup on my machine gets roughly 0.8 seconds faster (I did no real benchmark), which is quite an improvement.

# Dynamic AppCds

This blog should be about how to apply AppCds in a final product where you can't ask users to run a specific sequence of commands or run a script. Also the nature of Milkman (no installs but overwriting application for updating it, drag&drop plugins) leads to changes in both the classpath and the actuall classes (due to updates), which invalidates any generated AppCds cache.
Also I cannot precompile the AppCds cache and ship it, as this will nearly double the size of the shipped artifact.

Having this situation leads us to a list of requirements:
1. The AppCds cache should be dynamically compiled on the user-machine.
2. If something is wrong with the AppCds cache, just go on as usual, dont fail application startup
3. If anything invalidates the cache, it should be automatically recreated.

## 1. Dynamic Creation of AppCds Cache
For creating AppCds Cache, you need to have a list of classes that should be part of the AppCds cache. As this wont change too much, it can already be precompiled and put into the shipped artifact. It might not contain classes used in "unknown" plugins, so this might be something to compile dynamically as well.
The appCds cache can be generated by executing the according java-command, which is done in Milkman on a separate thread during the startup, if necessary.

## 2. Transparent AppCds cache
If something is off with the AppCds cache, -Xshare:auto flag will lead to the cache being ignored and the application being started in a normal way.

## 3. Regenerating an invalid AppCds cache
Now, this is the hard part. How to identify if the current application got loaded via AppCds or if the appCds is actually invalid and was not used during startup. I could only find one reliable way to see, if a Cache got used: try using it again in a sub-process, but with -Xshare:on, which leads to the application being terminated on startup, if the AppCds cache could not be used. Besides other things (like comparing, if the classpath that was used for creating the cache changed), this is exactly what is used in Milkman (see code).

If it shows that the cache is actually invalid, the file has to be replaced. This sounds easier than it is, as the file might be locked (in case where classpath changed, the file is not locked, but in case of class-changes, it is). Renaming still works (on Windows), so it will be renamed and deleted on next run.
