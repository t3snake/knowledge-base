---
tags:
 - c_programming
 - handmade
---

## Windows APIs
### Day 1
WinMain is the entry point for Windows apps which are non cli, most other editors using main is for cli apps which input and output to cli and start there.  
[API](https://learn.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-winmain)  

### Day 2
in WinMain we make a window using `RegisterClassA`, it takes a `WNDCLASS` struct instead of many parameters. one of the parameters `lpfnWndProc` we care about is callback which reacts to windows messages, and windows can call at other times.

In the `Win32MainWindowCallback` we can overwrite default behavior such as close (WM_CLOSE), resize, destroy create, paint and so on. The default behavior can be called usually with `DefWindowProc` with all the parameters of the callback.  

In WinMain, if RegisterClass is successful we create window using `CreateWindowEx` (just `CreateWindow` with extra options). Docs say we can use CW_DEFAULT for coordinates.

### Day 3
We could use paint 1 pixel at a time but instead we ask windows for a backbuffer whose format we can satisfy to draw a pixel on the screen.  
