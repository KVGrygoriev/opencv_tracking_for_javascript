# OpenCVs Tracking module for building with emscripten

Not so long ago I needed to compile C++ library which uses OpenCV 3.1.1 (Tracking module, particularly TrackerKCF algorithm) with Emscripten to run it inside a browser.
As it turned out there no Tracking module for OpenCV.js...So I had to build my own OpenCV with with blackjack and hookers.

Actually I no need to have opencv.js file, since I'm compiling my own library with interfaces to JS, which uses OpenCV inside.
Therefore instruction limited only to building .a files which should be linked to the project.
Anyway if you need the opencv.js file I recommend to take last OpenCV version and cherry-pick [that commit](https://github.com/KVGrygoriev/opencv_tracking_for_javascript/commit/638d6cc79fd4528a8f715904e86c7d87344b5e92) to it. (Current version has an issues)  

If you need algorithm differ to TrackerKCF all what you need it's to mention it inside **tracker** section in **opencv-master/modules/js/src/embindgen.py**

## Requirements
- Linux (Theoretically Windows should work, but...it's Windows)
- [Emscripten](https://github.com/emscripten-core/emscripten)
- Python

## How to build

- git clone git@github.com:KVGrygoriev/opencv_tracking_for_javascript.git
- cd opencv_tracking_for_javascript/opencv-master
- emcmake python platforms/js/build_js.py build_wasm --build_wasm --extra_modules ../opencv_contrib-master/modules/

Here error will appear such as
```
[ 98%] Linking CXX static library ../../lib/libopencv_dnn.a
[ 98%] Built target opencv_dnn
[ 98%] Generating bindings.cpp
Scanning dependencies of target opencv_js
[100%] Building CXX object modules/js/CMakeFiles/opencv_js.dir/bindings.cpp.o
/mnt/d/RnD/opencv_js_with_tracking/opencv-master/build_wasm/modules/js/bindings.cpp:481:18: error: reference to 'index' is ambiguous
        .element(index<0>())
```
As I said it's an issue related to my OpenCV version, therefore if you want generate custom opencv.js just download another OpenCV version.
Anyway **libopencv_plot.a** and **libopencv_tracking.a** already built and they located in **build_wasm/lib/**
