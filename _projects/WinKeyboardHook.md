---
title: 'WinKeyboardHook'
collection: projects
excerpt: 'Intercept and redirect keypresses'
parmalink: /projects/WinKeyboardHook
tags:
- CSharp
- WPF
---

## WinKeyboardHook
[Github Repo](https://github.com/huffSamuel/WinKeyboardHook.WPF)

[Nuget Package](https://www.nuget.org/packages/AobD.WinKeyboardHook.WPF/)

This application is a WPF adaptation of the excellent [Open.WinKeyboardHook](https://github.com/lontivero/Open.WinKeyboardHook) by [lontivero](https://github.com/lontivero). This software behaves as a layer on top of the keyboard, routing keyboard events through itself to be handled by additional software. These keyboard events can then be trapped, prevenging Windows or other applications in the event pipeline from recognizing that a key was pressed.