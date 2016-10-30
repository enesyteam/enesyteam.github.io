---
layout: post
title:  "ObjectARX là gì?"
desc: "Tìm hiểu về một giao diện lập trình mạnh mẽ nhất dành cho AutoCAD."
keywords: "objectarx,enesy"
date: 2016-10-01
categories: [ObjectARX]
tags: [objectarx]
icon: fa-file-code-o
---

`ObjectARX` (AutoCAD Runtime eXtension) is an API for customizing and extending AutoCAD. The ObjectARX SDK is published by Autodesk and freely available under license from Autodesk.[1] The ObjectARX SDK consists primarily of C++ headers and libraries that can be used to build Windows DLLs that can be loaded into the AutoCAD process and interact directly with the AutoCAD application. ObjectARX modules use the file extensions .arx and .dbx instead of the more common .dll.
ObjectARX is the most powerful of the various AutoCAD APIs, and the most difficult to master. The typical audience for the ObjectARX SDK includes professional programmers working either as commercial application developers or as in-house developers at companies using AutoCAD.

New versions of the ObjectARX SDK are released with each new AutoCAD release, and ObjectARX modules built with a specific SDK version are typically limited to running inside the corresponding version of AutoCAD. Recent versions of the ObjectARX SDK include support for the .NET platform by providing managed wrapper classes for native objects and functions.
The native classes and libraries that are made available via the ObjectARX API are also used internally by the AutoCAD code. As a result of this tight linkage with AutoCAD itself, the libraries are very compiler specific, and work only with the same compiler that Autodesk uses to build AutoCAD. Historically, this has required ObjectARX developers to use various versions of Microsoft Visual Studio, with different versions of the SDK requiring different versions of Visual Studio.
Although ObjectARX is specific to AutoCAD, Open Design Alliance announced in 2008[2] a new API called DRX (included in their DWGdirect library) that attempts to emulate the ObjectARX API in products like IntelliCAD that use the DWGdirect libraries.