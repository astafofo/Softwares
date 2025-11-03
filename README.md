# Release DLL — consumer artifact

Small repo containing a single prebuilt **Release** DLL (and optional `.lib` / headers) intended to be used by another C++ project. No source or host code is included.

## Contents
- `releases/` — `MyLib.dll` (Release). May include `MyLib.lib` and `MyLib.h` if provided.

## Usage (short)
- **Link-time**: add `MyLib.lib` to your linker inputs and ship `MyLib.dll` alongside your executable.
- **Runtime/dynamic load**: use `LoadLibrary` / `GetProcAddress` (Windows) or `dlopen` / `dlsym` (POSIX) to load and call exported functions.

Example (Windows):
```cpp
#include <windows.h>
typedef int (__stdcall *Fn_DoWork)(int);

HMODULE h = LoadLibraryA("MyLib.dll");
if (h) {
    Fn_DoWork doWork = (Fn_DoWork)GetProcAddress(h, "DoWork");
    if (doWork) {
        int r = doWork(42);
        // ...
    }
    FreeLibrary(h);
}
