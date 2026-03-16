---
title: Control Master
description:
weight: 10
---


## SSH ControlMaster

> [!NOTE]
> This feature is not supported on Windows.


The client can leverage SSH ControlMaster mode to share multiple SSH sessions over a single network connection.

This improves connection speed and avoids repeating the authentication process for each session.
