# mingw-w64-benchmark
mingw arch linux build.

## Quick Start - Command Prompt
```bash
mkdir mingw-w64-benchmark
cd mingw-w64-benchmark
wget https://raw.githubusercontent.com/cgenrich/mingw-w64-benchmark/master/PKGBUILD
yaourt -Pi .
```

## Hacks applied
Case insensitive windows builds often make use of **#include <>** statements that follow W32 API guidelines but may not be supported by all compiler environments.  
### Modified:
* Windows.h
* Shlwapi.h
* VersionHelpers.h.
```bash
prepare() {
  find ${_sourcename} -name *.cc -exec sed -i -e "s/Windows.h/windows.h/g" -e "s
/Shlwapi.h/shlwapi.h/g" -e "s/VersionHelpers.h/versionhelpers.h/g" {} \;
}
```
### After CMake
CMake will incorrectly use case-insensitive compiler flags and other configuraiton.  Replace Shlwapi after cmake but before make.
```bash
find ../.. -exec sed -i "s/Shlwapi/shlwapi/g" {} \; -print 2>/dev/null
```
