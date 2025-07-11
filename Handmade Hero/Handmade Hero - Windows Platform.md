---
tags:
 - c_programming
 - handmade
---

# Windows Entry Point

## Create a Window
#Day_1_Handmade  
WinMain is the entry point for Windows apps which are non cli, most other editors using main is for cli apps which input and output to cli and start there.  
[API](https://learn.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-winmain)  

#Day_2_Handmade  
in WinMain we make a window using `RegisterClassA`, it takes a `WNDCLASS` struct instead of many parameters. one of the parameters `lpfnWndProc` we care about is callback which reacts to windows messages, and windows can call at other times.

```
Note: WNDCLASS class = {} // initializes everything in struct to 0
```

### Message Loop
Use `GetMessage` or `PeekMessage` to get message from windows such as close, resize and so on and it is up to us to handle it in the callback function that windows calls, if our program doesnt do anything special then we can use `DefWindowProcA` to handle it.

`TranslateMessage` and `DispatchMessageA` should be used to handle messages. Dispatch calls the callback defined at a later point.

In the `Win32MainWindowCallback` we can overwrite default behavior such as close (WM_CLOSE), resize, destroy create, paint and so on. The default behavior can be called usually with  with all the parameters of the callback.  

`PostQuitMessage` with exit code should be called so that wm_quit is appended to message queue and is called once the rest of the queue is handled and then the program quits.

### Creating a window

In WinMain, if RegisterClass is successful we create window using `CreateWindowEx` (just `CreateWindow` with extra options). Docs say we can use CW_DEFAULT for coordinates.  
If it is not NULL or non zero then the window was successfully created and can proceed.  
If zero or NULL in create or register window, it is a failure and some logging might be required. 

# Windows Painting API

### Painting on window
Window borders are painted by os, we are responsible for painting the client area.  
We wrap `BeginPaint` and `EndPaint` in our callback handling the WM_PAINT message. These functions signal to the os that we begin and end our painting and then the OS takes over.  `BeginPaint` also gives us the device context `HDC` for further use.

We could use `PatBlt` to fill in paint in a rectange through x, y (top left coordinates) and w, h (the width and height of window). A `RECT` struct contains top bottom left right so we can calculate width and height from these.  

```
Note: static within a function will only initialize it once. It acts like a global variable that is only valid within scope.  
```

## Bitmap
#Day_3_Handmade  
```
Note: **Client** is the size within window borders, **Window** includes the border
We could use paint 1 pixel at a time but instead we ask windows for a backbuffer whose format we can satisfy to draw a pixel on the screen.  
```

```
Note: static has 3 meanings in different contexts:
- for a function it makes it so the function cant be called by name outside translation unit.
- inside a block any static member is essentially global but local scoped, its never initialized again
- for a global variable, initializes to 0, cant be accessed outside translation unit
```

DIB: Device independant Bits  
Each pixel has: R, G, B each 0 to 255 (8bits/1byte each)  
pixel: **BB GG RR xx** (little endian format reverses order) (xx is padding  bits to match boundary: 4byte/32 bits)  

We make it so that on resize we create a new `BITMAPMEMORY` and `BITMAPINFO`.
During paint we paint the bitmap into memory using `StretchDIBits` (alternative `BitBlt`). This copies a source rectangle to destination rectangle and stretches/compresses if there  is a size difference.  
StretchDIBits uses flag iUsage which is either DIB_RGB_COLORS (each pixel is RGB 32bit value) or DIB_PAL_COLORS which is a palette of limited colors and thus array of numbers representing the colors, could represent a pixel in less bits ( use in retro games )  

## Allocating Bitmap Memory
#Day_4_Handmade  
We can use stdlib and use malloc or new, instead we can also use os level allocation using `VirtualAlloc()` or `HeapAlloc()`. new or malloc would also call the same.  
Virtual Alloc uses `flAllocationType` to mention `MEM_COMMIT` (for immediate use) and `MEM_RESERVE` (allocate for future use) flags. `MEM_RESERVE` is mandatory, cant use commit without reserve.  
`flProtect` are the permissions for read write and execute for the allocated memory.  

```
Note: VirtualProtect is useful to prevent stale ptr, avoid "use after free" bugs.
```

Free memory using `VirtualFree()`. If we use `MEM_RELEASE`, there is no need to pass the size since OS remembers the size from when you used `VirtualAlloc()`.

## Drawing Pixels
Pitch or Stride is the distance of bits between 2 rows.  
2d grid of screen is represented by a linear sequence of memory.  

```
Note: char unsigned is long and we need to use it for 8bit/byte conversion a lot. in stdint.h they have defined convenient types such as uint8_t, uint32_t ...intr_t and so on.
```

Pixels are represented in XX RR GG BB where BB are the first 8 bits then green is next 8 bits and so on for single pixel being 32 bits.  
we could get a unsigned 8 bit ptr and do following:  
```C
*pixel = 208; //blue channel (0-255)
pixel++; // if pixel is unsigned char* ptr or uint8_t then ++ moves 8bits
*pixel = 0; // green channel
pixel++;
*pixel = 12; //red channel
pixel++
*pixel = 0; // padded bits, could be alpha channel?
```
Or we could do the following to set pixel in one go using bitwise operations:  
```C
uint8_t red = 12;
uint8_t green = 0;
uint8_t blue = 208;

*(pixel++) = (red << 16) | (green << 8) | (blue);
```

To paint/ blit in other places other than WM_PAINT, we likely need device context.
we can use`GetDC(window_handle, rect)` to get device context but **do not forget** to return the dc with `ReleaseDC(window_handle, rect)` (for loop with getDC call without releaseDC had a memory leak)


# Windows XInput API
A very simple API.
include `xinput.h` and load the library dll relevant (mostly 1.4)

To dynamically load dlls using function ptrs:
#c_programming Function pointers
```cpp
#define InputStateGetMacro(name) DWORD name(DWORD dwUserIndex, XINPUT_STATE* pState)
// defines a func ptr type with signature defined in prev statement
typedef InputStateGetMacro(fp_x_input_get_state); 
// create func ptr of the type
fp_x_input_get_state* XInputGetStatePtr 

// load a dll (searches in exe directory, ..., System32 directory, ...)
HMODULE xinput_lib_handle;
xinput_lib_handle = LoadLibraryA("Xinput1_4.dll");

// Get pointer from library and cast it to the pointer we defined
XInputGetStatePtr = (fp_x_input_get_state *) GetProcAddress(xinput_lib_handle, "XInputGetState");
```