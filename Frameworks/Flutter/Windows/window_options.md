# Window options

Under `windows/runner/main.cpp` you can change the initial window size:

```cpp
FlutterWindow window(project);
Win32Window::Point origin(10, 10);
// Set the below
Win32Window::Size size(500, 500);
```

To change other options like what buttons are shown on the app bar or if the window can be resized or not go to `windows/runner/win32_window.cpp` and find the WS arguments on the CreateWindow function.

```cpp
HWND window = CreateWindow(
      window_class, title.c_str(), WS_OVERLAPPED | WS_VISIBLE | WS_SYSMENU | WS_MINIMIZEBOX,
      Scale(origin.x, scale_factor), Scale(origin.y, scale_factor),
      Scale(size.width, scale_factor), Scale(size.height, scale_factor),
      nullptr, nullptr, GetModuleHandle(nullptr), this);
```

Following the table from Microsoft on Win32 apps we can see the flags and what they do, you’ll need to add or remove flags to get the desired window behaviour.

For more details: [Window Styles (Winuser.h) - Win32 apps](https://docs.microsoft.com/en-us/windows/win32/winmsg/window-styles)