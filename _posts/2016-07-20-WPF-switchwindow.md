---
layout: post
title: Switch to another window using C#
description: "C# tricks"
tags: [C#]
categories: [Tech, Program]
---

{% raw %}

```c#
[DllImport("user32.dll")]   
public static extern void SwitchToThisWindow(IntPtr hWnd,bool turnon);
String ProcWindow = "wechat";
private void switchToWechart()
{
    Process[] procs = Process.GetProcessesByName(ProcWindow);
    foreach (Process proc in procs)
    {
         //switch to process by name
         SwitchToThisWindow(proc.MainWindowHandle, true);
    }
}
```
{% endraw %}



