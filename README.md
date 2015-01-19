# TouchVG

TouchVG is a lightweight 2D vector drawing framework for iOS, Android and Windows.

**NOTE**: The project has been split into three projects: [vgios](https://github.com/rhcad/vgios), [vgandroid](https://github.com/rhcad/vgandroid) and [vgwpf](https://github.com/rhcad/vgwpf).
Please visit the related project to get newest code.

Features described in [Online document](http://touchvg.github.io).

![arch](http://touchvg.github.io/images/arch.svg)

![iphone1](/doc/images/iphone1.png) | ![android1](/doc/images/android1.png) | ![iphone2](/doc/images/iphone2.png)

## License

This is an open source [LGPL 2.1](LICENSE.md) licensed project. It uses the following open source projects:

- [TouchVGCore](https://github.com/rhcad/vgcore) (LGPL): Cross-platform vector drawing libraries using C++.
- [svg-android](https://github.com/japgolly/svg-android) (Apache): Vector graphics support for Android.
- [SVGKit](https://github.com/SVGKit/SVGKit) (MIT): Display and interact with SVG Images with CoreAnimation on iOS.
- [simple-svg](http://code.google.com/p/simple-svg) (BSD): A C++ header file for creating SVG files.
- [rapidjson](https://github.com/Kanma/rapidjson) (MIT): A fast JSON parser/generator for C++ with both SAX/DOM style API.
- [x3py](https://github.com/rhcad/x3py) (Apache): Compile script files.
- [SWIG](https://github.com/swig/swig) (GPL): Use the tool to generate the glue code for Java and C#.
- [iOS-Universal-Library-Template](https://github.com/michaeltyson/iOS-Universal-Library-Template): Use it to create static library project.
- Algorithms: [NearestPoint.c](http://tog.acm.org/resources/GraphicsGems/gems/NearestPoint.c), 
[Bezier's bound box](http://processingjs.nihongoresources.com/bezierinfo/#bounds), 
[The intersection of two circles](http://blog.csdn.net/cyg0810/article/details/7765894), 
[Position judgment](http://orion.math.iastate.edu/burkardt/c_src/orourke/tri.c)
 and [Fitting digitized curves](https://github.com/erich666/GraphicsGems/blob/master/gems/FitCurves.c).

## How to Contribute

Contributors and sponsors are welcome. You may translate, commit issues or pull requests on this Github site.
To contribute, please follow the branching model outlined here: [A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/).

## Contributors

- [Zhang Yungui](https://github.com/rhcad)
- [Archer](https://github.com/a7ch3r)
- [ljlin](https://github.com/ljlin)
- [Pengjun](https://github.com/pengjun) / Line and triangle commands
- [Proteas](https://github.com/proteas)

# How to Compile

## Compile for Android

- Import all projects under `./android` directory of TouchVG in eclipse, then build  `touchvg` project.

  - Android SDK version of the projects may need to modify according to your installation.
  - Recommend using the newer [ADT Bundle](http://developer.android.com/sdk/index.html) to avoid complex configuration.

- You can download the [prebuilt libraries](https://github.com/rhcad/TouchVGTest/archive/android_prebuild.zip) from TouchVGTest and extract to `yourapp/libs`.

-  To regenerate libtouchvg.so, please enter `android` directory of TouchVG, then type `./build.sh`
(Need to add the [NDK](http://developer.android.com/tools/sdk/ndk/index.html) installation location to your PATH environment variable).

   - Type `./build.sh -B` to rebuild the native libraries.
   
   - Type `./build.sh APP_ABI=x86` to build for the x86 (Intel Atom) Emulator.
   
   - If the error `build/gmsl/__gmsl:512: *** non-numeric second argument to wordlist function` occurs, then open the `build/gmsl/__gmsl` file in the NDK installation directory, and change line 512 to:
     `int_encode = $(__gmsl_tr1)$(wordlist 1,$(words $1),$(__gmsl_input_int))`

   - MSYS and TDM-GCC(a MinGW distribution) are recommended on Windows.

   - To regenerate the kernel JNI classes, type `./build.sh-swig`
(Need to install [SWIG](http://sourceforge.net/projects/swig/files/), and add the location to PATH).

- How to debug native code
  - Add `#include "mglog.h"` and use `LOGD("your message %d", someint);` in C++ files needed to debug.
  - Set LogCat filter in Eclipse: `tag:dalvikvm|AndroidRuntime|vgjni|touchvg|vgstack|libc|DEBUG`.
  - NDK JNI log print to locate problems of libc:
    1. Add `python addlog.py` in [TouchVG/jni/build.sh](android/TouchVG/jni/build.sh).
    2. Type `./build.sh -swig`, or remove `touchvg_java_wrap.cpp` and type `./build.sh`.

## Compile for iOS

### Compile with CocoaPods

TouchVG is available on [CocoaPods](http://cocoapods.org). Just add the following to your project Podfile:

```ruby
pod 'TouchVG'
```

Or use the develop version:

```ruby
pod 'TouchVG', :podspec => 'https://raw.githubusercontent.com/touchvg/TouchVG/develop/TouchVG.podspec'
```

Or add the following to use SVG rendering feature with [SVGKit](https://github.com/SVGKit/SVGKit):

```ruby
pod 'TouchVG-SVG', :podspec => 'https://raw.githubusercontent.com/touchvg/TouchVG/develop/ios/TouchVG-SVG.podspec'
```

Then type `pod install` or `pod install  --no-repo-update`. Need to remove `libPods-TouchVG.a` or `libPods-TouchVG-TouchVG-SVG.a` from Link Binary With Libraries. Remove `-lxml2` from Other Linker Flags for TouchVG-SVG target.

### Compile without CocoaPods

Alternatively, you can build as one of the following methods:

- Open `TouchVG.xcworkspace` in Xcode, then build the `TouchVG` or `TouchVG-SVG` target.

   - `libTouchVG.a` does not support SVG display.
   - `libTouchVG-SVG.a` can display SVG shapes using [SVGKit](https://github.com/SVGKit/SVGKit).

- Or enter `ios` directory and type `./build.sh` to compile static libraries to the `ios/output` directory.
  - Type `./build.sh -arch arm64` to make for iOS 64-bit.
  - Type `./build.sh clean` to remove object files.

## Compile for Windows

- Open `wpf/Test_cs10.sln` in Visual Studio 2010 (Need VC++ and C#). Or open `wpf/Test_cs9.sln` in VS2008.

- To regenerate `wpf/touchvglib/core/*.cs`, please enter `wpf` directory and type `./build.sh`
(Need to install [SWIG](http://sourceforge.net/projects/swig/files/), and add the location to PATH).

## Compile for other platform

- You can compile TouchVG for Python, Perl or Java applications on Linux, MinGW or Mac OS X.

  - Enter `core` directory which contains Makefile, then type the following make command:

     - `Make all install`: compile C + + static library .
     - `Make java`: Jar package and generate dynamic libraries for Java programs.
     - `Make python`, `make perl`: namely Python, Perl , etc. to generate class files and dynamic libraries.
     - `Make clean java.clean python.clean`: delete these temporary files compiled out .

   - MSYS and TDM-GCC(a MinGW distribution) are recommended on Windows.
 
# Add more shapes and commands

- You can use [newproj.py](https://github.com/rhcad/DemoCmds/blob/master/newproj.py) to create library project containing your own shapes and commands. So the TouchVG and TouchVGCore libraries does not require changes.

  - Checkout and enter [DemoCmds](https://github.com/rhcad/DemoCmds) directory, then type `python newproj.py YourCmds`:

     ```shell
     git clone https://github.com/rhcad/DemoCmds.git
     cd DemoCmds
     python newproj.py MyCmds
     ```
    
  - Need to install python to run the script.