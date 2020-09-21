# Pin 2.14 For CSE141pp-Tool-Moneta

These modified Pin files are based on Pin 2.14, as newer versions of Pin make it very difficult to compile Pintools with external libraries, such as
Moneta's HDF5 library. More information about the compiliation issue can be found here:

https://chunkaichang.com/tool/pin-notes/

## Usage

General Pin usage information can be found on Intel's Pin 2.14 User Guide:

https://software.intel.com/sites/landingpage/pintool/docs/71313/Pin/html/

In order for Pin to compile using the Makefiles, the `PIN_ROOT` environment variable must be set to the directory containing the `pin.sh` file.

In order for Pin 2.14 to run on newer version of Linux, you will also need to include `-injection child -ifeellucky` as flags.
```
$PIN_SH_FILE -injection child -ifeellucky -t <your_tool.so> -- <executable>
```


## Makefile Modifications

The following Makefiles are located in the `source/tools/Config/makefile.unix.config` directory. A copy of these Makefiles with the modifications included can be found in the `modified_makefiles` directory.
  
### makefile.default.rules
The following lines are added under Line 166 of `makefile.default.rules` to include the C++ library for HDF5.
```
166: ###### Default build rules for tools ######
167:
168: TOOL_CXXFLAGS += -I/usr/include/hdf5/serial
169: TOOL_LPATHS += -L/usr/lib/x86_64-linux-gnu/hdf5/serial
170: TOOL_LIBS += -lhdf5 -lhdf5_cpp
```

### makefile.unix.config

Fixes the external library compilation issue with Pin.

Modifies Line 343 as follows:
```
<     TOOL_CXXFLAGS_NOOPT += -DTARGET_IA32E -DHOST_IA32E -fPIC
---
>     TOOL_CXXFLAGS_NOOPT += -DTARGET_IA32E -DHOST_IA32E -fPIC -fabi-version=2 -D_GLIBCXX_USE_CXX11_ABI=0
```
