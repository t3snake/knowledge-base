#Day_10_Handmade

# Preprocessor Method
One way to handle different platforms differently is to use preprocessors `#if` `#else` `#endif` and use a variable such as Handmade_Win32 that can be set in compiler to compile different platform versions.

```cpp
#if HANDMADE_WIN32
 int WinMain(...) {
#elif HANDMADE_Linux
 int main(int argc, char **argv) {
#endif
 // other code
 }

// in compiler use -DHANDMADE_WIN32=1 in compiler to build for windows
```

Problems with this approach is:
- Idea of Platform API is separation of concerns so someone else can just see the Win platform layer and port it to other platforms. This approach would mix all platform code.  
- Make the code very messy since each place would require this preprocessor. And editor  support would be required to fold the preprocessor.  
- This would require equivalent control flow for each platform. Each platform might have very different flows for startups and services etc. Eg: message loop in windows vs something else in linux.  

# Separate Platform Files as entry point
Abstracting different parts of platforms into a common interface (header file) and the game will just call common interface.  
```cpp
// in header file
void *PlatformLoadFile(char* file_name);

// in windows platform file (win32_handmade.cpp)
void *PlatformLoadFile(char* file_name){
  // windows specific code
}

// in linux platform file (linux_handmade.cpp)
void *PlatformLoadFile(char* file_name){
  // linux specific code
}

// The compiler will actually only compile either for windows or for linux so for each platform there will only be one implementation. Different from OOP kind of interfaces.
```

# Virtualizing the OS for the game
**Note**: we wont do it this way.  